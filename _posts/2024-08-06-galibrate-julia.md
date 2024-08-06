---
layout: post
title: Embedding Julia in a Python-based Genetic Algorithm Optimizer using PyJulia
subtitle: Implementation and Performance in GAlibrate version 0.7
tags: [software, Genetic Algorithm, optimization, Python, Cython, Numba, Julia, PyJulia]
---


<div class="alert alert-info">
<strong>TL;DR:</strong> Version 0.7.1 of GAlibrate is available and includes a new Julia integration using PyJulia: https://github.com/blakeaw/GAlibrate. The Julia version outperformed the standard Python version for cases of high-dimensional functions and large population sizes, but the earlier accelerated versions using Cython and Numba consistently outperformed the Julia/PyJulia integrated version, likely due to the added overhead associated with PyJulia conversions for a large number of calls to individual functions like the mutation and crossover operations.  
</div>


## Introduction


Hi! After a little more than four years, I've finally released a new version of the [GAlibrate](https://github.com/blakeaw/GAlibrate) software, version 0.7, for [Genetic Algorithm optimization](https://en.wikipedia.org/wiki/Genetic_algorithm) in Python. Although it has been a long time coming, this version adds several new features such as an automated test suite and a `resume` function that allows users to continue the optimization routine for additional generations after an initial call to the `run` function. However, the feature I want to talk about here is the new backend implementation of the optimizer that uses [Julia](https://julialang.org/) via [PyJulia](https://pyjulia.readthedocs.io/en/latest/) to boost performance, at least for more extensive problems (more on this later).


<div class="alert alert-info">
I just discovered while writing there is a relatively recent warning on the PyJulia GitHub repository's README noting that the ongoing development of functionality to call Julia code from Python as in PyJulia has transitioned to a new package: PythonCall.jl/juliacall https://github.com/JuliaPy/PythonCall.jl
So, you might use that in place of PyJulia.  
</div>


I started working on the Julia implementation of the genetic algorithm just around a year ago, after discovering PyJulia, which can be used to call and execute Julia functions from Python code. From work on a [previous post](https://blakeaw.github.io/2019-09-20-numba-vs-julia/) comparing Python to Julia, I knew that numerically intensive code ported to Julia had the potential to seriously outperform Python, even when the latter is accelerated with Numba. So, I decided it could be fun to implement Julia integration in the GAlibrate software.


As recounted in a [previous post](https://blakeaw.github.io/2019-09-05-galibrate/), GAlibrate started as a learning project. At the time, I wanted to get some practice implementing Python performance-enhanced code using tools like [Cython](https://cython.org/). I also had a latent interest in genetic algorithms, so I decided to work on implementing one of my own as a case study for learning Cython, which ultimately turned into implementing three backend versions of the optimizer: one in standard Python (with libraries like NumPy), one in Cython, and one using [Numba](https://numba.pydata.org/). Since GAlibrate had already served as my playground for trying out and learning new ways to boost the performance of numerically intensive Python routines, it seemed to be a natural choice for trying out the Julia/PyJulia integration.




## Implementation


In this case, I ported some of the critical genetic operation functions and some other utility functions to Julia, such as the crossover and mutation operations. Benchmarking of the standard Python version of the backend showed that `mutation` and `crossover` operations were among the most time-consuming operations ([see this notebook](https://github.com/blakeaw/GAlibrate/blob/master/notebooks/01_profile-performance.ipynb) for the full benchmarking results). Here are the code snippets for each of the four versions (Python, Cython, Python+Numba, Julia) of the `mutation` function:


<script src="https://gist.github.com/blakeaw/f7b15d1bd0d1554046e5c1dcf0a85a0f.js"></script>


Then, in the corresponding `run_gao_julia` module, I replaced the calls to the ported functions' Python counterparts with those implemented using Julia through PyJulia. The ported functions were translated to Julia in a separate Julia module: `gaofunc.jl`.


 As an example, the code snippet for applying the mutation function looks like:
```python
chromosomes = Main.mutation(chromosomes, locs, widths, pop_size, n_sp, mutation_rate)
```
where `Main` is the Julia instance, `from julia import Main`, into which the ported functions have been loaded, `Main.include('gaofunc.jl')`. For comparison, the standard Python version of the call to the mutation function is just:
```python
mutation(chromosomes, locs, widths, pop_size, n_sp, mutation_rate)
```
So, overall, the process was relatively straightforward to implement.


## Performance comparison


A more comprehensive performance analysis is available in [this notebook](https://github.com/blakeaw/GAlibrate/blob/master/notebooks/02_benchmark-parametric-scaling-performance.ipynb). For purposes of highlighting the performance comparison here, I'll just show the first result from that analysis comparing the average execution time across varying dimensionality of the optimized function (i.e., number of unknown parameters in the model) for each of the optimizer backends:


![image](https://drive.google.com/thumbnail?id=1UK933wjnGSi51YbtbTfM5xOHBlcrSPoe&sz=w400)


In this plot, smaller y-axis values are better (i.e., faster execution times). We can see that the Julia integrated backend (red) is only quicker than the standard Python version when the number of parameters is roughly >100. However, in all cases, the Cython and Numba versions remain the fastest. And, it's interesting to note that the Julia version is slower than the standard Python version with smaller numbers of parameters (i.e., less than roughly 100), presumably dominated by the overhead associated with calling the functions from PyJulia. Even so, for this case, the total execution times are less than a second for the Julia, Cython, and Numba versions across the tested dimensionalities, so the difference likely wouldn't be noticeable unless you need to rerun the optimization over and over many times. However, this may not be true for more complicated problems with even more computationally intensive fitness functions.


## Closing thoughts


In this case, implementing some of the bottleneck functions in Julia had mixed results. The backend with Julia/PyJulia integration outperforms the standard Python one by 3-4x for high-dimensional functions and large population sizes. However, it is still less performant than the Cython and Numba versions. This may not be the best use case, or my approach may not be the best way to implement the integration. Perhaps a strategy like porting the optimizer almost entirely into Julia and then using the [PythonCall.jl] interface to call the user-defined `fitness` function from Python could work even better.


Regardless, converting some functions to Julia and calling in Python using PyJulia was relatively easy. In some cases, like the one I demonstrated in [this previous post](https://blakeaw.github.io/2023-11-16-pyjulia-pimc/), it can yield excellent performance enhancement. So, I feel it remains a valuable tool to include in your arsenal of ways to boost numerically intensive Python code.


Well, that's all I have to say for now. Thanks for reading, and have a nice day! Feel free to message me if you have any questions or want to discuss. I'm also open to feedback and suggestions for future posts, so if there is something you think I should cover let me know! You can reach me by [email](mailto:blakeaw1102@gmail.com) or feel free to hit me up on [LinkedIn](https://www.linkedin.com/in/blakewilson3/).




Lastly, please share this post with anyone else who might be interested. It is a massive help to me, as it increases the blog's visibility and can help more people discover my work. Also, be sure to come back for future posts.




Until next time -- Blake


------


Like this content? You can follow this blog and get updated about new posts via my blog's RSS/Atom Feed.


------


If you are so inclined, you can also be a financial supporter of my open-source work through Ko-fi:




 [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/J3J4ZUCVU)


------


**Other related posts you might like:**


[Harnessing the speed of Julia in Python with PyJulia](https://blakeaw.github.io/2023-11-16-pyjulia-pimc/)


[Python+Numba vs. Julia](https://blakeaw.github.io/2019-09-20-numba-vs-julia/)


[Accelerating Python code with Cython and Numba](https://blakeaw.github.io/2019-09-05-galibrate/)

