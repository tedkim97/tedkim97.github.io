---
layout: post
title: CUDA Acceleration of Movie Barcodes
subtitle: Leveraging the RAPIDS api
published: true
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
  - bottlenecks
curated:
  - better_barcode_vis
  - cuda_barcodes
  - gaitkeep
  - numpy_dt_quirks
concepts:
  - CUDA
  - cuML
  - k-means
  - bottlenecks
---

# Introduction
[Around a year ago I tried making a movie visualization that expressed color usage in movies with K-Means]({% link _posts/2020-06-12-movie_vis_presentable.md %}) and [two weeks ago I complained about the run time & the work needed to set up the necessary libraries]({% link _posts/2021-03-04-cuda_barcodes.md %}). 

Well after booting my Ubuntu partition and [installing the conda package & environment management system](https://conda.io/projects/conda/en/latest/index.html#)[^1], I carried through the feature as expected. [I've made the changes on the repository here.](https://github.com/tedkim97/movie_visualizations)

[^1]: I normally dislike using python package managers besides `pip`, but `cuML` requires `conda`, so I just gave up and installed it. 

## TL:DR
The TLDR is that `cuML` **does** make the process faster (on my 1080ti gpu) without sacrificing quality. I was able to get "around" a \~1.4x to \~1.7x speed up using cuML (the cuda-accelerated machine learning library), that can be a pretty decent chunk of time given how long the process is. In addition, there are probably more things I can try to squeeze out performance.

# Managing Expectations with Benchmarks
Not all algorithms translate to a parallel algorithm as well. For instance, while [K-NN might get a 600x speed up](https://medium.com/rapids-ai/accelerating-k-nearest-neighbors-600x-using-rapids-cuml-82725d56401e), KMeans might not see the same speedup. That's because there's a limitation to parallelization (Ahmdal's Law). While the context is important, there are situations where the marginal speedup one can receive from an additional processor or thread starts becomes smaller than its cost, or a single bottleneck provides a limit for the fastest the program to run. 

As a result, I can't get a sense of the results I'm expecting unless I have a reference. [Fortunately, the `cuML` repository provides jupyter notebooks benchmarking/profiling the speedup you get from](https://github.com/rapidsai/cuml/tree/branch-0.20/notebooks/tools). For instance, while Regressions 
and PCA might be able to get 3x to 15x speedup, other algorithms get a more moderate 1.1x or 1.2x speedup. 

In the benchmarks (with me increasing the number of times the tests are performed from 3 to 10), the GPU version of KMeans is only moderately ranging from from 1.2x to 1.9x depending on the number of samples and points. 

```bash
KMeans (n_samples=16384, n_features=32) [... speedup=1.9462526740714778]
KMeans (n_samples=16384, n_features=256) [... speedup=1.491131772086756]
KMeans (n_samples=32768, n_features=32) [... speedup=1.300243724205509]
KMeans (n_samples=32768, n_features=256) [... speedup=1.294589758368672]
KMeans (n_samples=65536, n_features=32) [... speedup=0.743911355578058]
KMeans (n_samples=65536, n_features=256) [... speedup=1.2148675975984062]
```

These benchmarks seem promising, but when you adjust the parameters to fit our use-case, the results are concerning. At most we have \~130,000 pixels (`1920 (width) * 1080 (height) * 0.25 (width downsample) * 0.25 (height downsample)`), and we have only 3 features (Red, Green, Blue color channels). So while we can run faster with less features, we also run slower with more samples. For `n_features=3`, the benchmark are a bit inconsistent.  

```bash
KMeans (n_samples=131072, n_features=3) [... speedup=0.9742968538141555]
KMeans (n_samples=131072, n_features=32) [... speedup=1.4848135670925144]
KMeans (n_samples=262144, n_features=3) [... speedup=3.849639985106167]
KMeans (n_samples=262144, n_features=32) [... speedup=1.027466288654923]
```
Regardless, I'm going to have to face the reality that the speed increases might be marginal or zero.

# The Results
Adding `cuML` was easy and needed minimal rewrites because its mimics the `scikit-learn` API). The process wasn't adjustment free[^2] but straightforward. I was worried that some of those adjustments would cause some big approximation difference/distortions, but there seems to be no difference in quality. 

[^2]: for some reason I had to convert an image from `np.uint8` to be `np.float32` to work with cuML

{% assign caption1 = "KMeans movie barcode (processed with CUDA acceleration) on Coco" %}
{% include caption_image.html imgpath="/figures/kmeans_vis_coco_cuda.png" alt="coco barcodes with cuda" caption=caption1 %}

{% assign caption2 = "KMeans movie barcode (processed with just the CPU) on Coco" %}
{% include caption_image.html imgpath="/figures/kmeans_vis_coco_regular.png" alt="coco barcode without cuda" caption=caption2 %}

{% assign caption3 = "KMeans movie barcode (with CUDA) on Spider-Man: Into the Spider-Verse" %}
{% include caption_image.html imgpath="/figures/kmeans_vis_spiderman_cuda.png" alt="spiderman: into the spider-verse with cuda" caption=caption3 %}

{% assign caption4 = "KMeans movie barcode (without CUDA) on Spider-Man: Into the Spider-Verse" %}
{% include caption_image.html imgpath="/figures/kmeans_vis_spiderman_regular.png" alt="spiderman: into the spider-verse without cuda" caption=caption4 %}

Furthermore, I timed how long each version of the process took to compute everything. Out of my two comparisons, there was a \~1.4x to \~1.7x speedup.

|        Movie (n = 1)              |      CPU Run Time      |      GPU Run Time      | Speed Up  | 
|:--------------------------------: |:----------------------:|:----------------------:|:---------:| 
| Coco                              | 309 minutes 11 seconds | 177 minutes 41 seconds | 1.74x     | 
| Spider-Man: Into the Spider-Verse | 311 minutes 22 seconds | 217 minutes 5 seconds  | 1.43x     |  


# Monitoring Usage
Using `htop` (a process viewer) and `nvidia-smi` (nvidia's management CLI), I was able to monitor the usage/strain on my computer. Running the `cuML` version of the visualization process saturated 1-2 cores at \~92 - 100% usage, and kept the GPU usage at \~94-98%. The memory usage was (predictably) small, using a fixed amount around \~500mb. The `sklearn` version used all 8 cores of my CPU. If you're curious why `cuML` even uses so much of the CPU, it's because the movie is decoded using the CPU[^3].

[^3]: Probably. This is a reasonable conclusion I came to with some light test, but I should 100% confirm this by using a performance profiler, or replacing openCV's video decoder with a GPU based decoder. 

| Library Backend | 100% CPU Core Usage (out of 8) | System Memory Usage | GPU Usage  | GPU Memory Usage  | 
|:--------------: |:-----------------------------:|:-------------------:|:----------:| :----------------:|
| `scikit-learn`  | 8                           | (forgot to record)  | 0%         | 0                 |
| `cuML`          | 2                           | (forgot to record)  | 95% - 99%  | 531mb             | 


The **rigorous** way to measure usage is to use a logger with a performance profiler, and grab statistics rather than hand-wavy snapshots/maxes. However, since this isn't my job and I don't have infinite time, I'll stick to this now.


# Dealing with Bottlenecks 

In the end, I'm not sure if I fully utilized the accleration from CUDA. While the speedup falls within expectations, it's hard to **know** if I'm fully utilizing the hardware. For example, some (predictable) bottlenecks might be increasing the process length. [Like I mentioned in my excuse blog post]({% link _posts/2021-03-04-cuda_barcodes.md %}), there were some bottlenecks that relating to processing movie such as the path data takes: `storage -> memory -> GPU -> memory`, and the other requires decoding frames from the movie file. While I'm not sure I can solve the first solution easily, there may be a way to decode frames using the GPU and keeping them in the GPU for subsequent processing. Another solution may be to load `n` frames at a time, and run KMeans concurrently on the GPU.  Judging from my lazy metrics, the GPU memory usage was consistently low (a single, downsampled frame doesn't take that much memory). The GPU was maximizing its processing power, but I'm also unsure if that means every core on the GPU was running at 100%, or if a fraction of them were. 

# Conclusion
Mission success (?)! I got a decent speedup on a personal project of mine. The benchmarks weren't promising, but in my use case the visualization computation is faster. The bottlenecks may or may not be a problem, but we can address those later. 