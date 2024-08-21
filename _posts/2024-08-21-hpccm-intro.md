---
layout: post
title: Programming container recipes in Python with NVIDIA HPC Container Maker 
subtitle: A quick intro and case study combining Python and Julia
tags: [Python, Julia, NVIDIA, Containers, Docker, Singularity, HPC, PyJulia]
---

![image](https://drive.google.com/thumbnail?id=1nMiM3-N8nJLqLfD68QBBZAN7FnOePZt1&sz=w600)

Hello, and welcome!

A while back, I signed up for a course on _High-Performance Computing with Containers_ through NVIDIA's [Deep Learning Institute](https://learn.nvidia.com/) (note this course was discontinued in July and isn't available anymore). However, I only just recently finished working through the material presented in a computational notebook running in a GPU-enabled cloud computing instance. The course introduced me to NVIDIA's [HPC Container Maker](https://github.com/NVIDIA/hpc-container-maker) (or HPCCM). This open-source tool allows you to specify (HPC) container recipes as Python scripts, which HPCCM can then translate to Docker and Singularity container instruction/definition files. I think this is pretty cool because you only need to specify a single HPCCM recipe that can be used for both container platforms. I also appreciate the flexibility of the programmatic approach that allows you to embed other Python operations in your recipe if needed (e.g., parsing user input or reading data from files). 

In this post, I'll introduce you to HPCCM and apply it to generating container specifications for the [GAlibrate](https://github.com/blakeaw/GAlibrate) package. The most recent 0.7.z releases of this package have a new option for a Julia-accelerated backend, which I touched on in my [last post](https://blakeaw.github.io/2024-08-06-galibrate-julia/). Since it combines Python and Julia programming environments, I thought it would be a good scenario for containerization that could be used to demonstrate HPCCM.   


------

## Contents

1. What you need to follow along
2. Quick intro to HPC Container Maker
3. Defining our HPCCM recipe
4. Creating a Dockerfile and Building a Docker image
5. Testing out our Docker container.
6. Closing Thoughts
7. Acknowledgements

------

## What you need to follow along

If you want to follow along, you'll need to have **Python 3** with [hpccm](https://github.com/NVIDIA/hpc-container-maker/blob/master/docs/getting_started.md#installation) installed in your Python environment:

```
pip install hpccm
```

You'll also need [Docker](https://www.docker.com/), which can be installed from the docker website.  

### Computational Notebook Version

Content in this post was exported from a [Jupyter](https://jupyter.org/) IPython notebook, which is available at: [https://github.com/blakeaw/blog-posts/blob/new-post/hpccm-intro/notebooks/0003_hpccm-intro/post.ipynb](https://github.com/blakeaw/blog-posts/blob/new-post/hpccm-intro/notebooks/0003_hpccm-intro/post.ipynb)

------

## Quick intro to HPC Container Maker

As noted in the introduction, [HPC Container Maker](https://github.com/NVIDIA/hpc-container-maker) (or HPCCM)is an open-source tool that allows you to specify (HPC) container recipes as Python scripts. HPCCM can then translate the recipes to Docker and Singularity container instruction/definition files, so you only need to specify a single HPCCM recipe that can be used for both container platforms. Another nice thing about HPCCM is that it automatically applies various best practices in the generated container instruction files, so you don't have to worry about optimizing them yourself.

HPCCM has two primary components used to build recipes: _primitives_ and _building blocks_. [Primitives](https://github.com/NVIDIA/hpc-container-maker/blob/master/docs/primitives.md) are more basic operations such as base image specification and file operations (e.g., copying files into the container). [Building Blocks](https://github.com/NVIDIA/hpc-container-maker/blob/master/docs/building_blocks.md) are bundled instruction sets that take care of building/installing a specific tool, package, or library in the container, such as GNU compilers and OpenMPI.

There are two different ways you can use HPCCM. The simplest, and the way we will use it in this post, is to define a simplified recipe file (without all the boilerplate for importing primitives and building blocks and defining the stage objects) that is parsed by the `hpccm` command line interface (CLI) to generate a container specification. The second is to use the `hpccm` module (i.e., `import hpccm`), which gives you more programmatic flexibility but requires the user to manage input/output. You can check out the [Getting Started with HPC Container Maker](https://github.com/NVIDIA/hpc-container-maker/blob/master/docs/getting_started.md) page for more on the two different ways to use HPCCM.   

------

## Defining our HPCCM recipe

This section will cover making the HPCCM recipe for the [GAlibrate](https://github.com/blakeaw/GAlibrate) package. We want a container with Python and Julia programming environments so that GAlibrate can operate with its Julia-accelerated backend. I've already written the recipe in [hpccm_recipe.py](./hpccm_recipe.py), so let's take a look and then we will go over each part:


```python
!cat hpccm_recipe.py
```

    """HPCCM Recipe for GAlibrate with Julia backend acceleration.
    
    GAlibrate source:
    https://github.com/blakeaw/GAlibrate
    
    Usage:
    $ hpccm.py --recipe hpccm_recipe.py --format docker
    # hpccm.py --recipe hpccm_recipe --format singularity
    """
    
    # Choose a base image - Ubuntu
    Stage0 += baseimage(image="ubuntu:16.04")  # primitive
    
    # Install Conda with dependencies.
    Stage0 += conda(
        eula=True, packages=["python=3.10.11", "pip", "numpy=1.23.5", "scipy"]
    )  # building block
    
    
    # Install GAlibrate version 0.7.2 with Julia integration
    # and PyJulia (julia on PyPI)
    Stage0 += shell(
        commands=[
            ". ~/.bashrc", # Explicitly activate the bashrc
            "conda activate base", # Activate the conda environment
            "pip --no-cache-dir install https://github.com/blakeaw/GAlibrate/archive/refs/tags/v0.7.2.zip", # Install GAlibrate
            "pip --no-cache-dir install julia==0.6.1", # Install PyJulia
        ]
    )  # primitive
    
    # Install Julia programming environment - PyJulia needs
    # the PyCall.jl package, so we install that here.
    Stage0 += julia(version="1.9.2", packages=["PyCall"])  # building block
    
    
    

**1.**

The first step of any HPCCM recipe is specifying the base image we want for our container. I've used Ubuntu the most in recent years, so I opted for the Ubuntu 16.04 image available on Docker hub:

```python
Stage0 += baseimage(image="ubuntu:16.04")
```
Note that `baseimage` is an HPCCM _primitive_ component. `Stage0` represents the container stage we are adding all the components to. HPCCM allows you to write multistage recipes if needed, but in this case, it's just boilerplate since we aren't using multiple stages.  

------

**2.**

Next, I opted to use Conda to manage the Python environment in the container since that is what I typically use on my local machine. For this, we can use the `conda` building block:

```python
Stage0 += conda(
    eula=True, packages=["python=3.10.11", "pip", "numpy=1.23.5", "scipy"]
)
```
I used the optional `packages` list to specify the Python version (3.10.11) and other dependencies (pip, numpy, scipy). The `eula` option stands for End-User License Agreement; setting it to `True` means you accept it, which is required. HPCCM also has a `python` building block, but you can't pin the exact Python version it installs since it just pulls from the upstream Linux distro. 

------

**3.**

Then, I used the `shell` primitive to specify some additional shell commands to install GAlibrate and PyJulia:

```python
Stage0 += shell(
    commands=[
        ". ~/.bashrc", # Explicitly activate the bashrc
        "conda activate base", # Activate the conda environment
        "pip --no-cache-dir install https://github.com/blakeaw/GAlibrate/archive/refs/tags/v0.7.2.zip", # Install GAlibrate
        "pip --no-cache-dir install julia==0.6.1", # Install PyJulia
    ]
)
```

------

**4.**

Lastly, I used the `julia` building block to incorporate the Julia programming environment:

```python
Stage0 += julia(version="1.9.2", packages=["PyCall"])
```

I included `PyCall` in the optional `packages` list so it would installed in the Julia environment; `PyJulia` requires `PyCall` on the Julia side.

------

------

## Creating a Dockerfile and Building a Docker image

In this section, we'll start out by using the `hpccm` CLI to convert our recipe into a Dockerfile:


```python
!hpccm --recipe hpccm_recipe.py --format docker > Dockerfile
```

And, then we can view the contents of the Dockerfile:


```python
!cat Dockerfile
```

    FROM ubuntu:16.04
    
    # Anaconda
    RUN apt-get update -y && \
        DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
            ca-certificates \
            wget && \
        rm -rf /var/lib/apt/lists/*
    RUN mkdir -p /var/tmp && wget -q -nc --no-check-certificate -P /var/tmp http://repo.anaconda.com/miniconda/Miniconda3-py310_23.1.0-1-Linux-x86_64.sh && \
        bash /var/tmp/Miniconda3-py310_23.1.0-1-Linux-x86_64.sh -b -p /usr/local/anaconda && \
        /usr/local/anaconda/bin/conda init && \
        ln -s /usr/local/anaconda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
        . /usr/local/anaconda/etc/profile.d/conda.sh && \
        conda activate base && \
        conda install -y numpy=1.23.5 pip python=3.10.11 scipy && \
        /usr/local/anaconda/bin/conda clean -afy && \
        rm -rf /var/tmp/Miniconda3-py310_23.1.0-1-Linux-x86_64.sh
    
    RUN . ~/.bashrc && \
        conda activate base && \
        pip --no-cache-dir install https://github.com/blakeaw/GAlibrate/archive/refs/tags/v0.7.2.zip && \
        pip --no-cache-dir install julia==0.6.1
    
    # Julia version 1.9.2
    RUN apt-get update -y && \
        DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
            tar \
            wget && \
        rm -rf /var/lib/apt/lists/*
    RUN mkdir -p /var/tmp && wget -q -nc --no-check-certificate -P /var/tmp https://julialang-s3.julialang.org/bin/linux/x64/1.9/julia-1.9.2-linux-x86_64.tar.gz && \
        mkdir -p /var/tmp && tar -x -f /var/tmp/julia-1.9.2-linux-x86_64.tar.gz -C /var/tmp -z && \
        cp -a /var/tmp/julia-1.9.2 /usr/local/julia && \
        JULIA_DEPOT_PATH=/usr/local/julia/share/julia /usr/local/julia/bin/julia -e 'using Pkg; Pkg.add([PackageSpec(name="PyCall")])' && \
        rm -rf /var/tmp/julia-1.9.2-linux-x86_64.tar.gz /var/tmp/julia-1.9.2
    ENV LD_LIBRARY_PATH=/usr/local/julia/lib:$LD_LIBRARY_PATH \
        PATH=/usr/local/julia/bin:$PATH
    
    
    

Just to go ahead and demonstrate how easy it is to switch we can generate the Singularity version too (we just won't redirect the output to a file):


```python
!hpccm --recipe hpccm_recipe.py --format singularity
```

    BootStrap: docker
    From: ubuntu:16.04
    %post
        . /.singularity.d/env/10-docker*.sh
    
    # Anaconda
    %post
        apt-get update -y
        DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
            ca-certificates \
            wget
        rm -rf /var/lib/apt/lists/*
    %post
        cd /
        mkdir -p /var/tmp && wget -q -nc --no-check-certificate -P /var/tmp http://repo.anaconda.com/miniconda/Miniconda3-py310_23.1.0-1-Linux-x86_64.sh
        bash /var/tmp/Miniconda3-py310_23.1.0-1-Linux-x86_64.sh -b -p /usr/local/anaconda
        /usr/local/anaconda/bin/conda init
        ln -s /usr/local/anaconda/etc/profile.d/conda.sh /etc/profile.d/conda.sh
        . /usr/local/anaconda/etc/profile.d/conda.sh
        conda activate base
        conda install -y numpy=1.23.5 pip python=3.10.11 scipy
        /usr/local/anaconda/bin/conda clean -afy
        rm -rf /var/tmp/Miniconda3-py310_23.1.0-1-Linux-x86_64.sh
    
    %post
        cd /
        . ~/.bashrc
        conda activate base
        pip --no-cache-dir install https://github.com/blakeaw/GAlibrate/archive/refs/tags/v0.7.2.zip
        pip --no-cache-dir install julia==0.6.1
    
    # Julia version 1.9.2
    %post
        apt-get update -y
        DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
            tar \
            wget
        rm -rf /var/lib/apt/lists/*
    %post
        cd /
        mkdir -p /var/tmp && wget -q -nc --no-check-certificate -P /var/tmp https://julialang-s3.julialang.org/bin/linux/x64/1.9/julia-1.9.2-linux-x86_64.tar.gz
        mkdir -p /var/tmp && tar -x -f /var/tmp/julia-1.9.2-linux-x86_64.tar.gz -C /var/tmp -z
        cp -a /var/tmp/julia-1.9.2 /usr/local/julia
        JULIA_DEPOT_PATH=/usr/local/julia/share/julia /usr/local/julia/bin/julia -e 'using Pkg; Pkg.add([PackageSpec(name="PyCall")])'
        rm -rf /var/tmp/julia-1.9.2-linux-x86_64.tar.gz /var/tmp/julia-1.9.2
    %environment
        export LD_LIBRARY_PATH=/usr/local/julia/lib:$LD_LIBRARY_PATH
        export PATH=/usr/local/julia/bin:$PATH
    %post
        export LD_LIBRARY_PATH=/usr/local/julia/lib:$LD_LIBRARY_PATH
        export PATH=/usr/local/julia/bin:$PATH
    
    
    

Now that we have Dockerfile ready, we can generate our container image using the `docker build` command:


```python
!docker build -t hpccm/galibrate-julia .
```

    #0 building with "desktop-linux" instance using docker driver
    
    #1 [internal] load build definition from Dockerfile
    #1 transferring dockerfile: 1.93kB done
    #1 DONE 0.0s
    
    #2 [internal] load metadata for docker.io/library/ubuntu:16.04
    #2 DONE 0.9s
    
    #3 [internal] load .dockerignore
    #3 transferring context: 2B done
    #3 DONE 0.0s
    
    #4 [1/6] FROM docker.io/library/ubuntu:16.04@sha256:1f1a2d56de1d604801a9671f301190704c25d604a416f59e03c04f5c6ffee0d6
    ...
    ...
    ...
    #9 100.8   9 dependencies successfully precompiled in 33 seconds. 3 already precompiled.
    #9 DONE 101.5s
    
    #10 exporting to image
    #10 exporting layers
    #10 exporting layers 9.2s done
    #10 writing image sha256:2245059940e33abf9710e39c3f1c101eadcc9827a03ca226e5906482a7c7bbca done
    #10 naming to docker.io/hpccm/galibrate-julia done
    #10 DONE 9.2s
    
     
    View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/c81cwrpr08tgl2qmh50ji89h9
    [33m1 warning found (use --debug to expand):
    [0m - UndefinedVar: Usage of undefined variable '$LD_LIBRARY_PATH' (line 35)
    

And then check the list of images using the `docker images` command:


```python
!docker images
```

    REPOSITORY              TAG       IMAGE ID       CREATED         SIZE
    hpccm/galibrate-julia   latest    2245059940e3   9 seconds ago   3.05GB
    

-----

## Testing out our Docker container

Now that we have our Docker container image ready, we need to check that GAlibrate will run with Julia backend support. For this part, I used Powershell.

I executed the following call to `docker run` to start the container:
```docker
docker run -it --name hpccm-demo hpccm/galibrate-julia
```

![image](https://drive.google.com/thumbnail?id=1ha25ZpIQ_PUmbcWWqjtSWSdHltBa9PA7&sz=w600)


This will start the container and bring up an interactive shell in it.

Next, from the container shell I ran `python -c 'from galibrate import GAO'` to test importing the optimizer object (`GAO`) from GAlibrate:

![image](https://drive.google.com/thumbnail?id=1GhGKBPs_uBXcrgO_R90-wGlVtq-zNDEO&sz=w600)


This produced output:

![image](https://drive.google.com/thumbnail?id=1pzs9qvPxaITjnasX_k0nJP5IV0lojmtb&sz=w600)

We can see from the highlighted bit that we got the `RuntimeWarning` alerting us that the optimizer will run with Julia optimization, which means it is working as expected.

You can quit the container by issuing the `exit` command.

------

## Closing Thoughts

With its library of HPC building blocks, HPC Container Maker (HPCCM) is a great tool for programming container recipes in Python. It provides a flexible alternative to working directly with container specifications, and it can be used to generate container files in both Docker and Singularity formats. In this post, we reviewed an example HPCCM recipe for the GAlibrate package that combines Python and Julia programming environments. Then, we used the recipe to generate a Dockerfile, which we built and tested to ensure our container was successfully configured to allow the GAlibrate genetic algorithm optimizer to run with Julia acceleration. 

I hope this post was helpful for all you Python, containerization, and HPC enthusiasts out there. And, if you're not one of those but still found your way here, I hope you also learned something new!

Well, that's all I have to say for now. Thanks for reading, and have a nice day! Feel free to message me if you have any questions or want to discuss. I'm also open to feedback and suggestions for future posts, so if there is something you think I should cover, let me know! You can reach me by [email](mailto:blakeaw1102@gmail.com) or feel free to hit me up on [LinkedIn](https://www.linkedin.com/in/blakewilson3/).

Lastly, please share this post with anyone else who might be interested. It is a massive help to me, as it increases the blog's visibility and can help more people discover my work. Also, be sure to come back for future posts.

Until next time -- Blake

------

## Acknowledgements

Grammarly was used for proofreading and editing.

Source code images were generated using [carbon](https://carbon.now.sh/), and [GIMP](https://www.gimp.org/) was used for image editing. 

------
------

Like this content? You can follow this blog and get updated about new posts via my blog's RSS/Atom Feed.

------
------

If you are so inclined, you can also be a financial supporter of my open-source work through Ko-fi:

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/J3J4ZUCVU)

------
------

**Other related posts you might like:**

[Embedding Julia in a Python-based Genetic Algorithm Optimizer using PyJulia](https://blakeaw.github.io/2024-08-06-galibrate-julia/)

[Harnessing the speed of Julia in Python with PyJulia](https://blakeaw.github.io/2023-11-16-pyjulia-pimc/)

[Python+Numba vs. Julia](https://blakeaw.github.io/2019-09-20-numba-vs-julia/)

------
