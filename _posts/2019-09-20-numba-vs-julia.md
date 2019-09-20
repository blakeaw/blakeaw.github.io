---
layout: post
title: Python+Numba vs. Julia
subtitle: Monte Carlo estimation of Pi
tags: [Python, Numba, Julia, Monte Carlo, NumPy]
---

I was recently reminded of a Julia script I wrote several years back to estimate Pi using Monte Carlo sampling. I thought it was somewhat fortuitous, because I've been wanting to revisit the Julia language and get a little more programming experience with it. So, I decided I would I re-created code for a Julia function to estimate Pi using Monte Carlo sampling. And after my experience with accelerating Python code with Numba, partially recounted in a [previous post](https://blakeaw.github.io/2019-09-05-galibrate/), I also started wondering how Python with Numba acceleration would fair against similar Julia code. Numba adds Just In Time (JIT) compilation to Python code, similar to the JIT compilation used by Julia, which I thought would make for an interesting comparison. So, in this post I'll relay my timing results (code blocks included) for my comparison of Python+Numba and Julia code to estimate Pi using Monte Carlo sampling; additionally, for further comparison, I also ended up including Python-only and Python+NumPy versions of the Python function. Well, let's jump in.         

## Monte Carlo estimation of Pi

My PhD advisor taught me this problem. It is a relatively simple example of using Monte Carlo methods to estimate a quantity. The basic idea is we have the area of a circle, `Ac = Pi*r^2`, where in this case we assume `Pi` is an unknown proportionality constant that we want estimate. We first imagine inscribing a circle inside a square. Then we note that the ratio of the circle's area to that of the square is `Ac/As = Pi*r^2/(4*r^2) = Pi/4`. It's that ratio which we can estimate using Monte Carlo sampling. We simply draw samples at random within the square (uniform sampling) and check whether it falls within the circle. The fraction of samples within the circle, `f = n_circle/nMC`, is approximately equal to the ratio of the circle's area to that of the square it is inscribed within; `nMC` is the total number of Monte Carlo sample points.

# Code and timings
For the purposes of my timing tests I set `nMC = 100000000` and the function which runs the Monte Carlo sampling and returns the estimate of Pi is named `estimate_pi`

## Python+Numba implementation
First, we'll define and time the Python version of the `estimate_pi` function that uses the Numba `njit` decorator (with NumPy just for the random number generator):


```python
import numpy as np
import numba

@numba.njit
def estimate_pi(nMC):
    radius = 1.
    diameter = 2.*radius
    n_circle = 0
    for i in range(nMC):
        x = (np.random.random()-0.5)*diameter
        y = (np.random.random()-0.5)*diameter
        r = np.sqrt(x**2 + y**2)
        if r <= radius:
           n_circle += 1
    return 4.*n_circle/nMC

nMC = 100000000
for i in range(3):
    %time pi_est = estimate_pi(nMC)
    print(pi_est)
```

    CPU times: user 1.34 s, sys: 8 ms, total: 1.35 s
    Wall time: 1.35 s
    3.14133852
    CPU times: user 1.17 s, sys: 0 ns, total: 1.17 s
    Wall time: 1.17 s
    3.14166276
    CPU times: user 1.22 s, sys: 0 ns, total: 1.22 s
    Wall time: 1.22 s
    3.14162444


## Julia implementation
Second, we'll define and time the Julia version of the `estimate_pi` function:


```julia
%%script julia

function estimate_pi(nMC)
    radius = 1.
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

nMC = 100000000
for j in 1:3
    @time pi_est = estimate_pi(nMC)
    println(pi_est)
end    
```

    estimate_pi (generic function with 1 method)
    100000000
      0.387532 seconds (25.05 k allocations: 1.299 MiB)
    3.14161168
      0.370641 seconds (1 allocation: 16 bytes)
    3.14167128
      0.369961 seconds (1 allocation: 16 bytes)
    3.14157908


## Python-only and Python+NumPy versions
Lastly, as another point of comparison we'll redefine and time the Python version of the `estimate_pi` function without Numba decoration, as well as a Python version that uses NumPy vecotrized operations.

Here's the (naive) Python-only version (we still use NumPy for the random number generator):


```python
def estimate_pi(nMC):
    radius = 1.
    diameter = 2.*radius
    n_circle = 0
    for i in range(nMC):
        x = (np.random.random()-0.5)*diameter
        y = (np.random.random()-0.5)*diameter
        r = np.sqrt(x**2 + y**2)
        if r <= radius:
           n_circle += 1
    return 4.*n_circle/nMC

nMC = 100000000
for i in range(3):
    %time pi_est = estimate_pi(nMC)
    print(pi_est)
```

    CPU times: user 2min 49s, sys: 12 ms, total: 2min 49s
    Wall time: 2min 49s
    3.14170176
    CPU times: user 2min 48s, sys: 28 ms, total: 2min 48s
    Wall time: 2min 49s
    3.1415476
    CPU times: user 2min 41s, sys: 16 ms, total: 2min 41s
    Wall time: 2min 41s
    3.1414856


Here's the Python+NumPy version where we take better advantage of NumPy vectorization:


```python
def estimate_pi(nMC):
    radius = 1.
    diameter = 2.*radius
    xy = (np.random.random((nMC,2))-0.5) * diameter
    r = np.sqrt((xy**2).sum(axis=1))
    circle_mask = r <= radius
    n_circle = r[circle_mask].shape[0]
    print(n_circle)
    return 4.*n_circle/nMC

nMC = 100000000
for i in range(3):
    %time pi_est = estimate_pi(nMC)
    print(pi_est)
```

    78542459
    CPU times: user 4.3 s, sys: 1.44 s, total: 5.74 s
    Wall time: 5.8 s
    3.14169836
    78541573
    CPU times: user 4.55 s, sys: 1.55 s, total: 6.1 s
    Wall time: 6.14 s
    3.14166292
    78533044
    CPU times: user 4.87 s, sys: 1.6 s, total: 6.46 s
    Wall time: 6.48 s
    3.14132176


## Comparison
So, now we can compile the timing results. I'll list the best times in seconds:
  * Python+Numba: 1.17
  * Julia: 0.370
  * Python-only: 161
  * Python+NumPy: 5.8

As we can see the Julia version is the fastest, running at about a  


```python
# Python+Numba/Julia
1.17/0.370
```




    3.162162162162162



factor of 3 times faster than the Python+Numba version, over


```python
#Python-only/Julia
161/0.370
```




    435.13513513513516



400 fold faster than the (naive) Python-only version, and over


```python
# Python+NumPy/Julia
5.8/0.370
```




    15.675675675675675



ten times faster than the Python+NumPy version.

Now, the Python+Numba version is the second fastest. It was


```python
# Python-only/Python+Numba
161/1.17
```




    137.60683760683762



over a 100 fold faster than the Python-only version and roughly


```python
# Python+NumPy/Python+Numba
5.8/1.17
```




    4.957264957264957



five times faster than the Python+NumPy version.

#Summary
Although Numba increased the performance of the Python version of the `estimate_pi` function by two orders of magnitude (and about a factor of 5 over the NumPy vectorized version), the Julia version was still faster, outperforming the Python+Numba version by about a factor of 3 for this application. This is just one application relatively simple application, so it would be interesting to compare the performance for some more applications and see if Julia is consistently faster.  

Well, that's it for now. Thanks for reading. Feel free to share this post with anyone who might be interested. And be sure to come back for future posts; you can follow and get updates to this blog via its RSS/Atom Feed.

Until next time -- Blake
