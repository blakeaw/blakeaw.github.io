---
layout: post
title: Accelerating Python code with Cython and Numba
subtitle: My experience accelerating the core continuous genetic algorithm of the GAlibrate package
tags: [Python, Numba, Cython, genetic algorithm, optimization, model calibration]
---

Back in July, not long after I officially finished my postdoc, I decided to try writing a Python package that took advantage of either Cython or Numba to accelerate the core numerical algorithm(s); I wanted some more practical experience accelerating (numerically heavy) Python code (beyond NumPy). As my "real" problem I decided to code up a Genetic Algorithm (GA) optimizer, which ultimately turned into the [GAlibrate](https://github.com/blakeaw/GAlibrate) package.

Before getting started, I briefly looked into some other potential options for accelerating python code, such as [ctypes]()-based approaches, but I ultimately decided to try writing the GA in Cython. However, after writing a first (not particularly optimized) version of the core algorithm in Cython, I was curious to know how its performance would compare against a non-Cython version. So, I went ahead and ended up writing a second Numba version, and then a pure Python version (with some NumPy vectorization). I then worked on improving the performance of the Cython (type declarations, implementing memory views, etc.) and then Numba (making sure functions could compile in nopython mode, and setting Numba to cache compilations, etc.) versions of the core GA; many of the changes, such as porting new code blocks to functions, could be implemented in both accelerated versions of the code.

I found that after implementing those improvements (and learning some of the "tricks"), that I was able get the Cython and Numba versions to have comparable performance (the Numba version was typically a little faster than the Cython one), and I found that both accelerated versions of the GA code could achieve roughly an order of magnitude speed enhancement over the pure Python version in some cases (dependent on the problem size). In the end, my final impression was that Numba was much easier to use. Granted, I'm not a Cython (nor Numba) expert, but at least for this problem, I felt I was able to achieve better performance of the core GA with much less effort using Numba than what was required to achieve similar performance with the Cython version; with the Cython version, I spent a lot more time looking through the manual/tutorials, trying to get the little "tricks", and examining the annotated code. Regardless, I wanted to do a little more formal performance comparison of each of the GA versions (Cython, Numba, and pure Python) available in GAlibrate. The results (ported from the Jupyter notebook in the []() repo) are below.

## Performance comparison    
I'm working on getting a more thorough/detailed performance comparison set up and able to run in a Jupyter notebook. I'll come back and edit this post to add in the results after I've finished it, so be sure to come back later if your interested that data.


Well, that's all I have to say for now. Thanks for reading. Feel free to share this post with anyone who might be interested. And be sure to come back for future posts; you can follow and get updates to this blog via its RSS/Atom Feed.

Until next time -- Blake
