---
layout: post
title: Accelerating Python code with Cython and Numba
subtitle: My experience accelerating the core continuous genetic algorithm of the GAlibrate package
tags: [Python, Numba, Cython, genetic algorithm, optimization, model calibration]
---

Back in July, not long after I officially finished my postdoc, I decided to try writing a Python package that took advantage of either Cython or Numba to accelerate the core numerical algorithm(s); I wanted some more practical experience accelerating (numerically heavy) Python code (beyond NumPy). As my "real" problem I decided to code up a Genetic Algorithm (GA) optimizer, which ultimately turned into the [GAlibrate](https://github.com/blakeaw/GAlibrate) package.

Before getting started, I briefly looked into some other potential options for accelerating python code, such as [ctypes]()-based approaches, but I ultimately decided to try writing the GA in Cython. However, after writing a first (not particularly optimized) version of the core algorithm in Cython, I was curious to know how its performance would compare against a non-Cython version. So, I went ahead and ended up writing a second Numba version, and then a pure Python version (with some NumPy vectorization). I then worked on improving the performance of the Cython (type declarations, implementing memory views, etc.) and then Numba (making sure functions could compile in nopython mode, and setting Numba to cache compilations, etc.) versions of the core GA; many of the changes, such as porting new code blocks to functions, could be implemented in both accelerated versions of the code. Here is a code example which defines the function to generate the initial randomly chosen GA population:
**Cython**
```python
@cython.boundscheck(False)
@cython.wraparound(False)
cdef np.ndarray[np.double_t, ndim=2] random_population(int pop_size, int n_sp,
                      double[:] locs,
                      double[:] widths):
    cdef int i, j
    cdef np.ndarray[np.double_t, ndim=2] chromosomes = np.zeros([pop_size, n_sp], dtype=np.double)
    cdef np.ndarray[np.double_t, ndim=2] u = np.random.random([pop_size, n_sp])
    cdef double[:,:] chromosomes_v = chromosomes
    cdef double[:,:] u_v = u

    for i in range(pop_size):
        for j in range(n_sp):
            chromosomes_v[i][j] = locs[j] + u_v[i][j]*widths[j]

    return chromosomes
```    
**Numba**
```python
@numba.njit(cache=True)
def random_population(pop_size, n_sp,
                      locs, widths):
    chromosomes = np.zeros((pop_size, n_sp))
    u = np.random.random((pop_size, n_sp))

    for i in range(pop_size):
        for j in range(n_sp):
            chromosomes[i][j] = locs[j] + u[i][j]*widths[j]

    return chromosomes
```      
These code samples are of course the current versions which I arrived at after implementing those rounds of improvements. As you can see from the snippets, although the Numba version is cleaner because you don't have to add any type declarations, there really isn't that big of difference between that and the Cython version. In the end, I found that I was able to get the Cython and Numba versions to have comparable performance (the Numba version was typically just a little faster than the Cython one), and I found that both accelerated versions of the GA code could achieve roughly an order of magnitude speed enhancement over the pure Python version in some cases (dependent on the problem size). My final impression was that Numba had a much gentler learning curve and was overall a little easier to use; I would imagine even more so if you are not used to C-style type declarations. Granted, I'm not a Cython (nor Numba) expert and this was a learning experience for me, but I do have past experience writing C/C++ code, and at least for this problem, I felt I was able to achieve better performance of the core GA with less effort using Numba than what was required to achieve similar performance with the Cython version; with the Cython version, I spent a lot more time looking through the manual/tutorials, trying to get the little "tricks", and examining the annotated code.

## Performance comparison    
I wanted to do a little more formal performance comparison of each of the GA versions (Cython, Numba, and pure Python) available in GAlibrate. I'm currently working on getting a more thorough/detailed performance comparison set up and able to run in a Jupyter notebook, and I'll come back and edit this post to add in the results after I've finished it, so be sure to come back later if your interested that data.


Well, that's all I have to say for now. Thanks for reading. Feel free to share this post with anyone who might be interested. And be sure to come back for future posts; you can follow and get updates to this blog via its RSS/Atom Feed.

Until next time -- Blake
