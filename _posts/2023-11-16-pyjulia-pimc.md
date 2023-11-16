---
layout: post
title: Harnessing the speed of Julia in Python with PyJulia
subtitle: A case study in the Monte Carlo estimation of Pi
tags: [Python, Julia, PyJulia, Monte Carlo]
---

![image](https://drive.google.com/uc?id=1TVnJWb-HqociLFpClWGIGMtkHuMpR9HC) 

In a [previous post](https://blakeaw.github.io/2019-09-20-numba-vs-julia/), I compared different Python implementations of a function to estimate pi using Monte Carlo sampling to one in [Julia](https://julialang.org/). In that case, the Julia version outperformed all the Python implementations I tried. So, when I recently learned about [PyJulia](https://pyjulia.readthedocs.io/en/latest/index.html), which can be used to call and execute Julia functions from Python code, I wanted to try it out and see if I could harness the speed of Julia to boost the performance of one of my Python packages. After trying it out, seeing how easy it could be, and achieving decent results for my specific use case, I decided to share some of what I learned here to help you take advantage of integrating Julia code into your Python packages. 

In this post, we'll revisit the Monte Carlo estimation of pi and use it as a case study to illustrate how Julia can be used with Python and PyJulia to dramatically boost the performance of numerically intensive functions without sacrificing a Pythonic interface. Specifically, we'll adapt the function for estimating pi using Monte Carlo sampling from my [previous post](https://blakeaw.github.io/2019-09-20-numba-vs-julia/) into two versions: a basic Python implementation using only standard libraries and a Julia version, integrated into Python using PyJulia. Both implementations will be encapsulated within modules to mimic the structure of a distributable Python package. We'll also measure the execution times of both versions using a fixed number of Monte Carlo samples (100,000,000) and compare their relative performance.

------

## Contents

1. Setting Up Your Python and Julia Environment
2. Monte Carlo Estimation of Pi
3. Basic Python Implementation
4. Julia/PyJulia Implementation
5. Closing Thoughts
6. Acknowledgements

------

## Setting Up Your Python and Julia Environment

### Python
For this post, I'm using Python 3.10.11. I'm running inside a [conda](https://docs.conda.io/projects/miniconda/en/latest/) environment, but that's not absolutely necessary. However, if you want to run in a conda environment you can do:

```
conda create -n pimc-julia python=3.10.11
```

### Computational Notebook

If you want to follow along in your own Jupyter notebook you can install [JupyterLab](https://jupyter.org/):

```
pip install jupyterlab
```
I used version 4.0.5.

Or you could try [Google Colab](https://colab.google/).

### Julia

Julia downloads are available at [https://julialang.org/downloads/](https://julialang.org/downloads/). For this post, I used Julia version 1.9.2 installed using the Python utility [JILL](https://pypi.org/project/jill/) (version 0.11.5). 

```
pip install jill
```

#### JILL install

Installing Julia with JILL is pretty straightforward with a basic install using

```
jill install
```

JILL installs Julia in a particular location depending on your OS.

**Windows**

For Windows, the JILL install location is `~\AppData\Local\julias` with a `julia.cmd` symlink file in `~\AppData\Local\julias\bin`. Since this is not in your `PATH`, I defined a new environment variable `JULIA_RUNTIME` that points to `~\AppData\Local\julias\bin\julia.cmd` and used this to reference the Julia executable. This can be used later when setting up and when starting PyJulia. 

To start the Julia REPL from Powershell you can do:

```
iex $env:JULIA_RUNTIME
```

From Python, you can reference `JULIA_RUNTIME` using the `os` library:

```
import os

JLR = os.environ.get("JULIA_RUNTIME")
```

### PyJulia

PyJulia installation instructions are available at [https://pyjulia.readthedocs.io/en/latest/installation.html](https://pyjulia.readthedocs.io/en/latest/installation.html). 

#### If using `JULIA_RUNTIME` environment variable

If you are using the `JULIA_RUNTIME` environment variable (or some other location not in `PATH`) to specify the Julia executable then in step 3 in the Python REPL you can do:

```python
>>> import os
>>> JLR = os.environ.get("JULIA_RUNTIME")
>>> import julia
>>> julia.install(julia=JLR)
```

------

## Monte Carlo Estimation of Pi

Before jumping into the code, let's briefly review the problem of estimating Pi using Monte Carlo sampling. Assume we know Pi is related to the area of a circle _A<sub>circle</sub> = Pi r<sup>2</sup>_, but we don't know its value. To estimate Pi using the Monte Carlo method we can inscribe our circle in a square with side length _2r_ and area _A<sub>square</sub> = 4r<sup>2</sup>_, and then we can imagine throwing darts at random throughout the area of the square. If we tally the darts that land inside and outside the circle, we use that to estimate Pi. We do this by taking advantage of the relationship between the ratio of the areas of the circle and square and the ratio of darts inside the circle to the total number of darts thrown at the square (both inside and outside the circle):


![image](https://drive.google.com/uc?id=1ZR0cCtTWnsIMVUjSuC-OpXm8WeuT4fYW)

The more darts we throw, the more thoroughly we sample the two areas, and the more accurate our estimate of their ratio and thus Pi will be.

------

## Basic Python Implementation

We'll start with the basic Python implementation, creating our Python module named `pimc.py` with the following code:

```python
import random
import math


def run(nMC: int, radius: float = 1.0) -> float:
    """Estimates Pi using Monte Carlo sampling.
    """    
    diameter = 2.0 * radius
    n_circle = 0
    for i in range(nMC):
        x = (random.random() - 0.5) * diameter
        y = (random.random() - 0.5) * diameter
        r = math.sqrt(x**2 + y**2)
        if r <= radius:
            n_circle += 1
    return 4.0 * n_circle / nMC
```    

Then we can import the module here:


```python
import pimc
```

And, then set the number of Monte Carlo samples we want to draw in estimating pi:


```python
nsamples = 100000000
```

Next, we can time the `pimc.run` function using the notebook [%timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit) magic command:


```python
%timeit -n 1 -r 3 -o -q pimc.run(nsamples)
```




    <TimeitResult : 1min 2s ± 466 ms per loop (mean ± std. dev. of 3 runs, 1 loop each)>



We can see that on average this basic Python implementation takes about a minute to run with this number of samples.

-----

## Julia/PyJulia implementation

Now, we'll define and time the second version that uses Julia and PyJulia. 

First, we'll define the Julia implementation of the function in the module file named `pimc.jl` with code:

```julia
function run_pimc(nMC, radius)
    diameter = 2. * radius
    n_circle = 0
    for i in 1:nMC
        x = (rand() - 0.5) * diameter
        y = (rand() - 0.5) * diameter
        r = sqrt(x^2 + y^2)
        if r <= radius
           n_circle += 1
        end
    end
    return (n_circle/nMC) * 4.
end
```

Second, we'll define another Python module named `pimc_jl` with code:

```python
import os
# import the Julia Main namespace and
from julia import Main
# Now, include the custom Julia module.
MODPATH = os.path.dirname(os.path.abspath(__file__))
JLPATH = os.path.join(MODPATH, "pimc.jl")
Main.include(JLPATH)

def run(nMC: int, radius: float = 1.0) -> float:
    """Estimates Pi using Monte Carlo sampling.
    """
    
    return Main.run_pimc(nMC, radius)
```

In this version of the Python module we use PyJulia to include and call the `pimc.jl` module and the `run_pimc` function defined in it. I went ahead and wrapped the `Main.run_pimc` call with the `run` function to mimic the basic Python implementation's `run` function. 

Now, we can import the new Python module:


```python
%%capture
import pimc_jl
```

Here, we'll time an initial call to the Julia version. Be aware that Julia will compile the function on the first call, so there is some extra overhead for the initial call: 


```python
%time pimc_jl.run(nsamples)
```

    CPU times: total: 406 ms
    Wall time: 429 ms
    




    3.14176728



Even so, it is still significantly faster the the basic Python version.

Now, we'll more thoroughly benchmark the Julia version:


```python
%timeit -n 1 -r 3 -o -q pimc_jl.run(nsamples)
```




    <TimeitResult : 306 ms ± 21.3 ms per loop (mean ± std. dev. of 3 runs, 1 loop each)>



We can see that this version only takes about 306 milliseconds on average, which is around 200x faster than the basic Python implementation:


```python
# Basic Python / Julia version
62. / 306e-3
```




    202.6143790849673



That is a more than two orders of magnitude improvement!

### If using `JULIA_RUNTIME` environment variable

If you are using the `JULIA_RUNTIME` environment variable (or some other location not in `PATH`) to specify the Julia executable then you can add this additional code to the `pimc_jl.py`:

```python
# Extra boilerplate to handle a custom environment variable
# to set the location of the Julia runtime executable:
import warnings
# Import PyJulia and setup
# First, check for a custom Julia runtime
# executable via the JULIA_RUNTIME environment variable.
JLR = os.environ.get("JULIA_RUNTIME")
if JLR is not None:
    from julia import Julia

    JLR = os.path.abspath(JLR)
    warn_message = "Setting a custom Julia runtime from environment variable JULIA_RUNTIME: Julia(runtime={})".format(
        JLR
    )
    warnings.warn(warn_message, RuntimeWarning)
    Julia(runtime=JLR)
```

### What about overhead?

Since there is type conversion going back and forth between Python and Julia there may be some overhead associated with calling Julia functions from Python. To gauge this effect for our case study here, we'll time the Julia version of the Pi Monte Carlo function here when called directly from Julia.


```python
# If using the JULIA_RUNTIME environment variable, we'll
# add it's location to the PATH variable so we can more
# easily use the %%script magic command.
import os
import sys
sys.path.append(os.path.dirname(os.environ.get("JULIA_RUNTIME")))
```

Now, we'll use the `%%script` magic command to run Julia directly and time the `run_pimc` function defined in `pimc.jl`. Note that the location containing `julia.cmd` is now in our `PATH`. 


```python
%%script julia.cmd

include("pimc.jl")

function time_pimc()
    nsamples = 100000000
    radius = 1.0
    for j in 1:3
        @time run_pimc(nsamples, radius)
    end
    return
end

time_pimc()
```

    run_pimc (generic function with 1 method)
    time_pimc (generic function with 1 method)
      0.270891 seconds
      0.278231 seconds
      0.267313 seconds
    


```python
average_jl = (0.270891 + 0.278231 + 0.267313) / 3
average_jl
```




    0.272145



The Julia-invoked version takes roughly 272 ms, so calling the function in Python introduces an overhead of around 34 ms (~10-15% slower). While this overhead isn't really noticeable in this case, it's possible that it could accumulate and become more noticeable for frequently called functions. So, it's something to keep in mind, especially when considering which parts of your Python code you might port to Julia versus opting for other strategies like using [Numba](https://numba.pydata.org/) to accelerate Python functions.   

------

## Closing Thoughts

In this post, we covered an example of how Julia can be integrated into Python modules using PyJulia to dramatically boost the performance of numerically intensive functions. We revisited a case study I covered in a previous post [previous post](https://blakeaw.github.io/2019-09-20-numba-vs-julia/), estimating Pi using Monte Carlo sampling in a unit square. We compared the performance of a basic Python implementation and one where the core Monte Carlo routine was implemented in Julia and integrated back into Python using PyJulia. We found that the Julia-integrated version was a whopping two orders of magnitude (~200x) faster than the basic Python one!  

For all the Python enthusiasts interested in ways to maximize performance, I hope you’ve found this post interesting and helpful. However, I know we've only just scratched the surface here. So, if you want to see more examples of how to integrate Julia into Python, then I recommend checking out [this blog post by Peter Baumgartner](https://www.peterbaumgartner.com/blog/incorporating-julia-into-python-programs/). It is pretty extensive, and I found it a very helpful resource in getting started with PyJulia.

Now, it’s your turn. Go out and experiment more with integrating Julia into your own Python workflows. And, if you know someone else who might find this post interesting, please share!

Well, that’s it. Thanks for stopping by! If you have questions or comments, feel free to [email me](mailto:blakeaw1102@gmail.com) or hit me up on [LinkedIn](https://www.linkedin.com/in/blakewilson3/).

Until next time – Blake

Like this content? You can follow this blog and get updated about new posts via my blog’s RSS/Atom Feed.

------

## Acknowledgements

ChatGPT was used to generate some initial post title ideas, and Grammarly and ChatGPT were both used for proofreading and editing.

Source code images were generated using [carbon](https://carbon.now.sh/), and [GIMP](https://www.gimp.org/) was used for image editing. 

### Computational Notebook Version

This post was exported from a Jupyter IPython notebook, which is available at: https://github.com/blakeaw/blog-posts/blob/new-post/pyjulia/notebooks/0002_mcpi-pyjulia/post.ipynb