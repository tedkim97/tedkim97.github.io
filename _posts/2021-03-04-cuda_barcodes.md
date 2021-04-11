---
layout: post
title: Future plans for CUDA-accelerated KNN Movie Barcodes
subtitle: (I am not advertising for Nvidia)
published: true
enable_latex: false
permalink: cuda_barcodes
frontpage: false
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
  - gaitkeep
  - numpy_dt_quirks
concepts:
  - CUDA
  - k-means
  - data-loading
  - data visualizations
---

# Introduction
[Around a year ago I tried making a movie visualization that expressed color usage in movies with K-Means]({% link _posts/2020-06-12-movie_vis_presentable.md %}). While I'm proud of the product (or at least the process), a major flaw with the visualization is the amount of time spent creating the visualization. Holding the sampling rate fixed, the time to generate a "average color" or down-sampled frame" visualization took less than 5 minutes while the "KNN visualization" took around 10 **hours**. While I can live with that, I've been looking into ways to speed up the process.

{% assign caption1 = "An example of a KMeans movie barcode with 8 cluster run on Coco" %}
{% include caption_image.html imgpath="/figures/coco_movie_barcode_k8.png" alt="coco_barcode" caption=caption1 %}

# Previous Attempts @ Acceleration: CuPy
My goal was to speed up the visualization generation, but I also didn't want to spend a large amount of time developing a custom solution from scratch. I knew that neural network libraries received a lot of CUDA acceleration support, so I tried to accelerate processing using [`cupy`](https://cupy.dev/) - cuda-accelerated `numpy`. It was fairly easy to "rewrite" the code to use `cupy` instead of `numpy` because the library was designed for drop-in replacement in mind. Using `cupy` was a reminder why I should **profile the code** before trying to optimize anything - the new `cupy` functionality didn't decrease processing times because running K-means clustering on every `Nth` frame was the bottleneck (who would've guessed).

# New **Attempt**: RapidsAI's cuML
I trust that `scikit-learn` is already as optimized as can be, so I started looking into GPU-accelerated machine-learning libraries. [`cuML`](https://github.com/rapidsai/cuml) caught my eye because [I read this article speeding up KNN search by 600 times with cuML (written by someone on the team)](https://medium.com/rapids-ai/accelerating-k-nearest-neighbors-600x-using-rapids-cuml-82725d56401e) and this [stackoverflow post asking how to speed up KMeans](https://stackoverflow.com/questions/58346524/faster-kmeans-clustering-on-high-dimensional-data-with-gpu-support). `cuML` is a GPU (mainly CUDA) accelerated machine learning library[^0]. From my understanding, it's a subset of the [RAPIDS software libary](https://rapids.ai/about.html) that provides convenient abstractions to fit the data pipeline all in the GPU. 

[^0]: If you want to know *how* much faster `cuML` is, I can't tell you. I haven't tested these benchmarks for reasons explained above, but [there is a list of cuML benchmarks that you can run on their github](https://github.com/rapidsai/cuml/blob/branch-0.14/notebooks/tools/cuml_benchmarks.ipynb). 

There's actually little rewriting needed because `cuML` ["matches the API from scikit-learn"](https://github.com/rapidsai/cuml/blob/branch-0.19/README.md), and I use `scikit` heavily for these visualizations. This means that I should be able to just replace `sklearn.cluster.KMeans` with `cuml.cluster.KMeans`[^1].

[^1]: There probably needs to be code that handles moving from `host to device`, `device to host`, etc. I'm not the expert (yet)

```python
# Example of a (potential) drop in substitute
# from sklearn.cluster import KMeans
from cuml.cluster import KMeans 

#cluster_model = KMeans(n_clusters=num_clusters, init='k-means++', n_init=20) # cpu version
cluster_model = KMeans(n_clusters=num_clusters, init='scalable-k-means++', n_init=20) # gpu version

# snippet for producing a vis slice
cluster_model.fit(movie_frame.reshape((-1,3)))
_, counts = np.unique(cluster_model.labels_, return_counts=True)
max_ind = np.argmax(counts)
prom_color = np.around(cluster_model.cluster_centers_[max_ind]).astype('uint8')

```

## Minor (Motivational) Blockers
I emphasize the header of this paragraph as an **attempt** to use cuML because I've been stuck on "motivational blockers". "Motivational blockers" aren't technical issues or waiting for some spec, but rather issues related to environments/configurations or uncertainties that I don't have the energy to deal with after work. Even though I'm excited to implement and test these new changes, having to deal with environments or driver versioning (things critical to software, but not directly related to code) make it difficult to get the ball rolling. 

### OS Compatibility (As of March 4th, 2021 cuML v0.18.0 does not **directly** support windows)
`cuML` does have a windows build. I'm explicitly dating this because I don't want someone in 2027 calling me an idiot for not knowing all I had to do was call `pip install cuml-windows`. 

### WSL2 & CUDA Doesn't Work On My Machine 
To circumvent OS issues I usually use WSL for a quick, easy fix. Unfortunately configuring WSL and CUDA is a different story. I have read the [Nvidia's](https://docs.nvidia.com/cuda/wsl-user-guide/index.html), [Microsoft's](https://docs.microsoft.com/en-us/windows/win32/direct3d12/gpu-cuda-in-wsl), and [Ubuntu's](https://ubuntu.com/blog/getting-started-with-cuda-on-ubuntu-on-wsl-2) documentation. [I've read Microsoft's article on CUDA in WSL](https://docs.microsoft.com/en-us/windows/win32/direct3d12/gpu-cuda-in-wsl).

I have:
- Joined Windows Insider Program (and also gave telemetry permissions because I'm *forced* to) and set my install to the dev channel
- Installed WSL2, enabled it by default, installed Ubuntu 18.04, and confirmed it ran WSL2
- Uninstalled/Reinstalled CUDA drivers, tried different drivers, etc

I imagine the reason why CUDA and WSL2 doesn't work correctly is because I've done *terrible* things to my Windows install to get dual booting set up \~2 years. I'm not willing to give up on WSL yet because [there are fixes for people with issues similar to mine](https://github.com/microsoft/WSL/issues/6014#issuecomment-733185668), but at this point I would just boot some linux OS on my computer. 

### Movie Bottlenecks
Another motivational blocker is the *potential* bottlenecks using my GPU to accelerate computing and the optimizations needed to fix those. A classic ML bottleneck is the process of a CPU reading files/images from a storage device, loading them into memory, and then copying that memory into the child device (our GPU). In the context of this project, the CPU would read a frame of the movie from storage, load it into memory, and then the CPU would load the frame into the GPU. There's a 99% chance this bottleneck is **not** a problem for a project this small, but it's still something I need to benchmark regardless.

{% include caption_image.html imgpath="/figures/storage-cpu-gpu.svg" alt="fig1" caption="Data flow in a traditional workloads. Data flow from a storage device into the CPU, the CPU copies this data into memory, and then the CPU copies data in memory into the GPU." %}


Advancements in hardware & software [allow Nvidia GPU's to read from NVME storage rather than system memory](https://developer.nvidia.com/blog/gpudirect-storage/)[^2]. Even better, there are libraries that do these things for us! Unfortunately, these features are enterprise-tier which means 
it's only enabled for enterprise tier GPUs (I only have a consumer card[^3]) and supports enterprise file systems (WekaFS, DDN Exascaler, VAST)](https://docs.nvidia.com/gpudirect-storage/design-guide/index.html). 

{% include caption_image.html imgpath="/figures/storage-gpu.svg" alt="fig2" caption="Data flow in Nvidia's GPUDirect Storage. Data can flow from a storage device (needs to be PCIe connected) directly into the GPU. According to Nvidia, this alleviates bottlenecks caused by CPU I/O and redundant operations. Of course there are a lot of hardware and file system requirements (that I can't fulfill) to use this feature." %}

[^2]: I'm sure AMD's GPU and OpenCL has something equivalent - I'm just not technically competent enough to find it. 
[^3]: There's always the cloud

Suppose I *did* have an GPUDirect Storage enabled system, I would still have complications loading a movie from my mass storage device storage (HDD) onto a NVMe solid-state storage device (NVMe SSD), and then load it directly onto the GPU for compute using something like [`cudf`](https://github.com/rapidsai/cudf). The problem is that you can't just "read" arbitrary frames from a movie - these frames cannot just be "picked out" by some (`Video_Width x Video_Height x Color_Depth x Number_Of_Frames`) byte offset. I've learned that modern video codecs (the thing that decodes your video files) are complicated, and [parsing arbitrary frames relies on the frames that come before it](https://stackoverflow.com/a/22706622). 

It's not the end of the world. Some immediate work around solutions jump out to me:
1. Preprocess the target movie from the HDD and copy the sampled frames onto the NVMe SSD. (Potentially excessive read-write cycles, could lower the lifespan of our hardware. Not ideal since I'm not made of money)
2. Move the whole movie onto the NVMe device onto the GPU, and use the GPU to decode the codec (Potentially low-level, complicated, hard-to-fix code)

I feel like option `1` might be a better solution. I'm sure I'm missing some nuance, but since we need to write a movie onto the PCIe storage device anyways, if the sum all sampled frames is less than the movie (which it might not be) - it should write less. Furthermore, instead of writing the whole resolution frame of 1080p (1920x1080) or 4k (4096x2160), I could downsample the frame to a smaller resolution (which I do for the current version of the visualization) and reduce the data by `downsample resolution/original`.

# Conclusion
cuML is cool, but factors relating to my personal machine and uncertainty if this would **actually** make the vis faster make me lazy.  