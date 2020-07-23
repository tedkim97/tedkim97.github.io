---
layout: post
title: Movie Visualization Critique & Improvements &#58; Movie Barcodes 
subtitle: My critique of popular movie visualization subgenre (on reddit) and my take on the genre
published: true
enable_latex: false
permalink: /barcode_critique
frontpage: true
technical: true
funstuff: true
---

##### This is a shorter, more "deliverable" version of my writeup on my movie visualization, if you're interested in the methodology, reasoning, code commentary, background research click (I actually haven't done it yet)

# Introduction
There are a series of movie "barcode" visualizations that are fairly popular on the internet (I've seen them a lot on [reddit.com/r/dataisbeautiful](https://www.reddit.com/r/dataisbeautiful/)). An example that I like is ["The average color of every 10th frame of The Lion King (1994) from beginning to end"](https://www.reddit.com/r/dataisbeautiful/comments/d6l2d0/oc_the_average_color_of_every_10th_frame_of_the/).

{% assign caption1 = "An example of a popular movie visualization made by /u/SylphKnot" %}
{% include caption_image.html imgpath="https://preview.redd.it/3w2kfg8xhmn31.jpg?width=1024&auto=webp&s=90658c02b89e48c95964c35c1e126b0357afbdb5" alt=example1 caption=caption1 %}

While these barcodes are cool visualizations about the still images (frames) of a movie, I think these visualizations could show richer detail about the colors used in our movie. This post is about my *attempt* at making a visualization scheme that shows more about how color is used while preserving the artistic qualities.

# The Visualization
Since the critique and description of techniques are a bit long, let's start with looking at a few (alternative) proposals I have for movie visualizations. 

##### How to read:
Each one pixel wide (1px) column of this visualization represents a frame sampled from the movie while the colors in each column represent the "significant"[^1] colors extracted from the frames. Their height is proportional to how frequently the color was used in the frame (i.e the bigger the portion, the more frequently the color appeared).

A frame was sampled from the target movie every 24 frames, and the movies I processed were 23.98 frames per second, meaning each column approximates a second in the film.

[^1]: "Significant" as in colors extracted from a clustering algorithm.

### Labeled Barcode
<center><blockquote class="imgur-embed-pub" lang="en" data-id="QRhzs9m"><a href="https://imgur.com/QRhzs9m">see on imgur</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

### The Original Image Barcode
<center><blockquote class="imgur-embed-pub" lang="en" data-id="xYcu3Gz"><a href="https://imgur.com/xYcu3Gz">see on imgur</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

In addition to creating this psuedo-stacked-area color chart, I tried creating some visualizations derived from the original.

### An alternative visualization: The most frequent colors
This was created by taking the top pixel in each column of our original visualization, and creating a new one from it.
<center><blockquote class="imgur-embed-pub" lang="en" data-id="frmAQ3Q"><a href="https://imgur.com/frmAQ3Q">see on imgur</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

### An alternative visualization: the least frequent colors
This was created by taking the bottom pixel in each column of our original visualization.
<center><blockquote class="imgur-embed-pub" lang="en" data-id="FghC82P"><a href="https://imgur.com/FghC82P">see on imgur</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

### The colors of the movie shown with equal representation
Instead of representing colors by their frequency on the frame, we just represent them all equally. You'll notice some redundancies (as in very similar) in the colors chosen in some of the columns due to our color extraction method (more on that later).
<center><blockquote class="imgur-embed-pub" lang="en" data-id="aCfh7sW"><a href="https://imgur.com/aCfh7sW">see on imgur</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>


# Critique 
I already showed one example of a barcode visualization in the introduction. Below are two more popular examples (with their links). The titles usually explain the visualization, so I won't re-describe them to you.

{% assign caption2 = "made by /u/julekca" %}
###### ["Blade Runner 2049..... represented by 1600 captures of the movie. Each of these is resized to 1px wide and extracted with the same time interval."](https://redd.it/d7nw9p)
{% include caption_image.html imgpath="https://preview.redd.it/tkmqnoywz3o31.jpg?width=1024&auto=webp&s=e5294d737152749b3e389af16a0765f5551cf12d" alt="example2" caption=caption2 %}

{% assign caption3 = "made by /u/julekca" %}
###### ["The sleek colors of "Joker" by Todd Phillips. One circular line = one frame, for a total of 1600 frames extracted at regular intervals from the movie. Its beginning lies in the middle of the disc and spreads outwards"](https://redd.it/fmdhhr)

{% include caption_image.html imgpath="https://preview.redd.it/cykcrmp3f0o41.jpg?width=768&auto=webp&s=2f193147669ae7e10864da6e75ab3c6106f8aeb3" alt="example3" caption=caption3 %}

### Petty Complaints
Some complaints I have are minor. For example, authors describe sampling frames so that the movies are the same barcode length (i.e removing movie length), averaging colors for the sake of prettiness, or (seemingly) clipping out the bottom or top of the frame. These moves seemed to be done to make the visualization more focused on **art** rather than **data** - which is "okay". Movies are art, and a visualization about movies can be artistic as well. 

### Actual Critique
My two main critiques of these visualizations is that these visualizations **skew colors differently from human perception**, and they **don't visualize the variety of colors** in movies. 

Both average and interpolated visualizations are close to showing something much richer than average color or resized frames. 

For example, our lion king visualization shows an interesting use of colors over time, but fails to show the variety of colors over time. 

If we tried to make an analogy onto a different visualization, this is what the "averaging" barcode shows:
{% assign lp1 = "a lineplot" %}
{% include caption_image.html imgpath="https://www.mathworks.com/help/examples/graphics/win64/SpecifyLineWidthMarkerSizeAndMarkerColorExample_01.png" alt="lineplot1" caption=lp1%}

And this is what I would like to see:
{% assign lp2 = "a lineplot with error bars" %}
{% include caption_image.html imgpath="https://www.mathworks.com/help/examples/graphics/win64/ModifyErrorBarsAfterCreationExample_01.png" alt="lineplot2" caption=lp2%}

The Bladerunner visualization is able to show us color over time, but focuses on the content of the frame, rather than color. I ran an implementation of the barcode visualization described in the [blade runner post](https://redd.it/d7nw9p) on "The Simpson's Movie". If you look carefully at the slices over time - you can see slices of the character's faces - stitched together (like Homer and Marge) because the frame was centered on these characters at those times.

<center><blockquote class="imgur-embed-pub" lang="en" data-id="HKidawr"><a href="https://imgur.com/HKidawr">see on imgur</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

### Averaging doesn't represent colors well

Literally speaking, averaging colors represent mixing. For example averaging a half blue and half red image is like mixing red and blue paint. The average will result in purple. The more blue pixels we add, the more violet the mixture becomes and the more red pixels, the more pink it becomes. 

The problem is that this average is not natural for how humans perceive color. 

However, this average doesn't let us understand how "colorful" a movie is, or how a scene will employ a variety of colors at once.


My problem is that averages don't represent the variety of color (or variation).

Averages don't represent colors well. Although (in RGB color space) there can be high correlation between the R, G, B vectors - the variety of colors (and how randomly they appear) is not represented by averages.

<center><blockquote class="imgur-embed-pub" lang="en" data-id="a/DvFPIRy"><a href="//imgur.com/a/DvFPIRy">Examples of Averaging Colors in Images</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

----------------------------------------


Averages (can) represents data poorly. Extreme cases (like a skewed histogram or a U-shaped distribution) will have averages don't represent the underlying distribution at all. A common fix is to use a different metric (like the median) or include information about the variance or skew of the distribution. 

Even though these 1px slices seem to represent color schemes and motifs in the film, it doens't capture the beauty/variety of colors. **In a sense - they fail to show variance**


### Interpolation isn't about colors (but the center of the frame)
The other visualization (interpolation of each frame) seems to do a better job of showing variety and color - just peeking at the visualization, we can see that there is a lot of different colors on the x & y axis. 

The only problem is that this technique when resizing to (1, image_height) only provides a vertical slice of the image. 


Interpolation is the process of reducing the image size down (we see that the image is progressively squeezed).

Interpolation of the images don't actually end up focusing on the **colors** in the frame, but rather whats in the center.


Not the variety in the colors used in the frame. 

**For this one, I recommend actually going to imgur and zooming in to see the difference**

<center><blockquote class="imgur-embed-pub" lang="en" data-id="a/th0wXPm"  ><a href="//imgur.com/a/th0wXPm">Examples of interpolation on images</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

Using Interpolation is compressing frames rather than representing colors


### Concessions with Compression
Dealing with compression and lossy representations 

In this project i end up decreasing the sizes of the image processed on, and 

However, I think there is only a marginal loss in quality/information.

If I *really* wanted to experiment with the pure version, I would be stealing the tapes of these films from theaters. 

**If i wanted a pure representation, I would probably have used a lossless original version**


# Problems with MY visualization
I think the "1px" limit is to restraining, and doesn't allow for a careful examination of a movie. I think this 1 pixel width limit could be fixed by adding some interactivity (like zooming).

In addition, I think the original visualization is too confusing to explain in words. 

If I had more time, I'd grab a small portion of the image and explain the structure visually.

# The Gallery
Below is an imgur album with some of the visualizations I created with visualizations done on Coco, Mad Max: Fury Road, Despicable Me, Shrek, The Simpsons Movie, and Spider-Man: Into the Spider-Verse. In addition to picking different movies, I also chose a preset number of cluster centers to extract colors from (k=3, 5, 8).

There wasn't a huge rationale behind the choices of these movies, these were the movies that came to mind when I thought "movies with distinct colors".

<center><blockquote class="imgur-embed-pub" lang="en" data-id="a/9RQDEkD"  ><a href="//imgur.com/a/9RQDEkD">Colors of (proportional)</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>

### These visualizations are not a 100% representation of color
Issues with represneting colors in data
Issues with represneting colors when taking a photo
Averaging/compression can dampen colors
Down-sampling (which is what I had to do for this visualization) reduces details and can cause small shifts in colors
KMeans is still not a pure representation of colors

# Making Movies more Computable/ Computationally Tractable

# The Machine Learning Bit; Color Quanitization with K Means

# An Analysis of the some Visualizations
Off the bat, looking at a few of these images show that the *light* prevents us from getting

which isn't very surprising given how 5/6 of the movies chosen were animated.


### Comparisons: a short clip as an example
I think we can try the a clip from the "Stargate Sequence - 2001: A Space Odyssey" to capture a better idea.

https://youtu.be/ebmwYqoUp44?t=6

The clip is from (0:06 to 1:15).

Comparison of each image

<center><blockquote class="imgur-embed-pub" lang="en" data-id="a/f4asdaM"><a href="//imgur.com/a/f4asdaM">Space Odyssey Visualizations (sample_rate = 12, slice_width=16)</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></center>


# Conclusion & Next Steps

I'd like to apply a TF-IDF weighting scheme to this movie catalog - 

WHere colors (r,g,b) act as words and a "document" would be a movie. 

# FAQ
### What about the black bars?
The black bars aren't actually a part of the frame - for example a 1080p movie (1920 x 1080) will show as a (1920 x 800) frame or something similar.

### How did you source these movies?
yohoho and a bottle of rum

### Is this processing technique runnable on a laptop?
Probably not. I have a fairly beefy desktop and it took 7-8 hours to process a 2 hour movie.

### What about filtering out common colors (ex: removing (0,0,0) and (255,255,255) pixels)?
I've thought about it, and it's feasible to do. The problem is that it doesn't catch a fairly significant amount of pixels that aren't pitch black (0,0,0) but extremely close, such as (0,0,3) or (5,0,1).

### What about exploring with other color spaces (HSV, HSL, CIELab, etc?)  
I thought about it, and I am still looking into it. At the moment it seemed easier to work everything in a simple R, G, B vector. Even though alternative color spaces and conversions are *relatively* simple, it still takes a lot of effort adjusting. 

I've also thought about using k-means with an alternative distance metric, or maybe a distance metric specific to color (like a modern CIE distance metric). However, KMeans is not guaranteed to converge with any arbitrary measure of distance I choose. 

### What about an alternative clustering method?
I've thought about spectral clustering (but it doesn't seems overkill for this problem), and I'm aware of other clustering methods out there - but I have not experimented with other methods.


This way, we are able to emphasize/pickout colors that are unique to a movie, and ignoring excessively common colors (i.e (0,0,0), (255, 255, 255)) 

https://nateaff.com/2017/09/11/lego-topic-models/#:~:text=Color%20TF%2DIDF&text=In%20text%20mining%2C%20stop%20words,the%20majority%20of%20brick%20colors.

https://www.kaggle.com/nateaff/finding-lego-color-themes-with-topic-models

# Code & Stuff
**Insert link to the repo here**

# Sources & Links
##### Various Barcode Related Things
https://moviebarcode.tumblr.com/
https://thecolorsofmotion.com/
https://kottke.org/19/02/movie-color-palettes
https://github.com/jyotiska/movie-barcode/blob/master/movie_barcode.py
https://zerowidthjoiner.net/movie-barcode-generator
https://www.reddit.com/r/dataisbeautiful/comments/gc4mbi/oc_visualizations_of_movies_in_one_figure/
https://www.reddit.com/r/dataisbeautiful/comments/d6l2d0/oc_the_average_color_of_every_10th_frame_of_the/
https://www.reddit.com/r/dataisbeautiful/comments/d7nw9p/oc_blade_runner_2049_represented_by_1600_captures/
https://www.reddit.com/r/dataisbeautiful/comments/fmdhhr/oc_the_sleek_colors_of_joker_by_todd_phillips_one/
https://www.reddit.com/r/dataisbeautiful/comments/d7nw9p/oc_blade_runner_2049_represented_by_1600_captures/
https://www.reddit.com/r/dataisbeautiful/comments/d8ue6x/inspired_by_recent_blade_runner_barcode_both/


##### Machine Learning Related Things
https://scikit-learn.org/stable/auto_examples/cluster/plot_color_quantization.html#:~:text=In%20the%20image%20processing%20literature,example%2C%20uses%20such%20a%20palette.

https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html

http://charlesleifer.com/blog/using-python-and-k-means-to-find-the-dominant-colors-in-images/

https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_ml/py_kmeans/py_kmeans_opencv/py_kmeans_opencv.html

https://www.pyimagesearch.com/2014/05/26/opencv-python-k-means-color-clustering/

# Footnotes