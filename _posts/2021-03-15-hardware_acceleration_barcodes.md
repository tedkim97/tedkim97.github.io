---
layout: post
title: Hardware Acceleration of Movie Barcodes
subtitle: With CUDA
published: false
enable_latex: false
permalink: cuda_barcodes_implementation
frontpage: true
technical: true
funstuff: false
tags: 
  - vis
  - ml
  - data-sci
  - metrics
  - stats
  - movie
  - programming
  - python
  - numpy
  - art
  - cuda
curated:
  - better_barcode_vis
  - cuda_barcodes
  - gaitkeep
  - numpy_dt_quirks
concepts:
  - CUDA
  - k-means
  - data-loading
  - data visualizations
---

# Introduction
[Around a year ago I tried making a movie visualization that expressed color usage in movies with K-Means]({% link _posts/2020-06-12-movie_vis_presentable.md %}) and [two weeks ago I complained about the run time & the work needed to set up the necessary libraries]({% link _posts/2021-03-04-cuda_barcodes.md %}).

Well after just booting from my Ubuntu partition and [installing the conda package & environment management system](https://conda.io/projects/conda/en/latest/index.html#), I carried through using . 


I'm normally dislike using python package managers besides `pip`, but I just gave up and installed it. 

I've made the changes on the repository here.

## TL:DR
I was able to get "around" a 2x speed up (more like 1.8x) using cuML (the cuda-accelerated machine learning library), and there are probably more ways that I can squeeze out perfomrance

cuML **does** make the process faster (by a pretty significant amount) 



# Managing Expectations with Benchmarks

Not all algorithms benfit from parallelizing very well

Not all algorithms/accerlations are infinitely parallelizable. At some point, the marginal speedup one can receive from getting an additional process or thread starts not being irrelevant. 

As a result, I can get a sense of the results I'm expecting unless I have a refernce. 

Fortunately, the `cuML` repository provides jupyter notebooks benchmarking/profiling the speedup you get from .

# Clarification on SCI-KIT LEARN
sklearn does take advantage of the hardware you're running your machine at. 

I probably shouldn't have been lazy with this profiling...

using `htop` to monitor process and performance usage,


# Monitoring Usage

Using `htop` and `nvidia-smi`



## Non-Cuda
CPU usage:
8 cores \~92 - 100%
## Cuda
1-2 cores \~92-100%
GPU usage \~95-99%
GPU memroy \~ 531mb usage of the GPU

# Comparing Out

# Maxing out compute, but not maxing out memory 

95-97% processor usage (from nvidia-smi)
marginal memory usage (531mb)


# Easy-Implementation 
Fortunately, cuML's algoriths are almost completely drop in replacement for Sci-kit learn's API.

The process wasn't adjustment free (I had to convert an image from `uint8` type to be `float32` to be compatabile with cuML.

was some specific issues with proce

# Conclusion
cuML is cool, but factors relating to my personal machine and uncertainty if this would **actually** make the vis faster make me lazy.  