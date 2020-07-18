---
layout: post
title: Visualization Critique & Improvements &#58; Movie Barcode Visualizations
subtitle: My critique popular movie visualization subgenre (on reddit) and my take on the genre
published: true
enable_latex: false
permalink: /vis_critique
frontpage: true
technical: true
funstuff: true
---

**This is a much shorter version of my writeup on my movie visualization, if you're interested in the methodology, indepth coverage/reasoning, code commentary, go to here**

# Visualization
Before we get into the some methodology and the critique, below is the visualization.

Each 1px wide column of this visualization represents  

*Link MY visualizations here*

An alternative visualization is ones that use the most prominent colors (color that appears the most):

And another visualization is one that uses the least prominent colors (colors that appear the list):



# Introduction
This is an alternative movie visualization in the series of "movie barcodes" that are popular on [reddit.com/r/dataisbeautiful](https://www.reddit.com/r/dataisbeautiful/). While these barcodes are cool data visualizations about the frames of a movie[^1] - I think that there is some room to improve these visualizations to communicate more about colors (and how they're used) in the movies we process.

[^1]: Ignoring the ***incredibly*** commercial aspects of the visualizations...

# Critique 
Below are some embedded links to the visualizations, with their links. These are fairly straightforward visualizations, so I won't re-describe them to you.

{% assign caption1 = "made by /u/SylphKnot" %}
<center><b>"The average color of every 10th frame of The Lion King (1994) from beginning to end"</b></center>
{% include caption_image.html imgpath="https://preview.redd.it/3w2kfg8xhmn31.jpg?width=1024&auto=webp&s=90658c02b89e48c95964c35c1e126b0357afbdb5" alt=example1 caption=caption1 %}

{% assign caption2 = "made by /u/julekca" %}
<center><b>"Blade Runner 2049..... represented by 1600 captures of the movie. Each of these is resized to 1px wide and extracted with the same time interval."</b></center>
{% include caption_image.html imgpath="https://preview.redd.it/tkmqnoywz3o31.jpg?width=1024&auto=webp&s=e5294d737152749b3e389af16a0765f5551cf12d" alt="example2" caption=caption2 %}

{% assign caption3 = "made by /u/julekca" %}
<center><b>"The sleek colors of "Joker" by Todd Phillips. One circular line = one frame, for a total of 1600 frames extracted at regular intervals from the movie. Its beginning lies in the middle of the disc and spreads outwards"</b></center>

{% include caption_image.html imgpath="https://preview.redd.it/cykcrmp3f0o41.jpg?width=768&auto=webp&s=2f193147669ae7e10864da6e75ab3c6106f8aeb3" alt="example3" caption=caption3 %}


These visualizations feel more focused on **art** rather than **data**. There's nothing wrong with that - movies are art, and a visualization about movies can be artistic as well.

However, I feel like these visualizations are on the cusp of communicating something much richer and interesting than just. 


Some critiques are minor is that they arbitrarily normalize all these movies to the same length (i.e removing movie length), and averages colors for the sake of prettiness.

These visualizations opt to show variance in the visualizations through time (i.e like a scatter plot) 

### Averaging doesn't represent colors (well)
This is just a general critique that is applied to colors.

Even though these 1px slices seem to represent color schemes and motifs in the film, it doens't capture the beauty/variety of colors. **In a sense - they fail to show variance**


If we tried making an analogy onto a different visualization, this is what the "averaging" barcode shows:
{% assign lp1 = "a lineplot" %}
{% include caption_image.html imgpath="https://www.mathworks.com/help/examples/graphics/win64/SpecifyLineWidthMarkerSizeAndMarkerColorExample_01.png" alt="lineplot1" caption=lp1%}

And this is what I would like to see:
{% assign lp2 = "a lineplot with error bars" %}
{% include caption_image.html imgpath="https://www.mathworks.com/help/examples/graphics/win64/ModifyErrorBarsAfterCreationExample_01.png" alt="lineplot2" caption=lp2%}

Averages are a perfect representation of color, and accurately represent our understanding of colors. EX Mixing Pure Red and Green gives us [COLOR], Pure Blakc and Pure White gives us Grey.

Averaging gives us a perfect representation of mixing colors. If we averaged an image that's half red and half blue, it would give us purple. 


Similarly, if there were more red than blue in the image, the picture would take a redder shade (just like mixing paint would).

The problem is that this average is not natural for how humans perceive color. 


*** SOME VARIATION OF THE SAME THING BELOW ***
However, this average doesn't let us understand how "colorful" a movie is, or how a scene will employ a variety of colors at once.


My problem is that averages don't represent the variety of color (or variation).

Averages don't represent colors well. Although (in RGB color space) there can be high correlation between the R, G, B vectors - the variety of colors (and how randomly they appear) is not represetned by averages.

*** END**** 

The other visualization (Bicubic downsampling of each frame) seems to do a better job - displaying the average of the rows in each image, it only provides averages of the rows in the frame, but not the variety in the colors used in the frame. 

### Using Interpolation is compressing frames rather than representing colors

### Minor Critique: Normalizing movies to the same time length 

# Requirements

# Problems with MY visualization
I think the "1px" limit is to restraining, and doesn't allow for a careful examination of a movie. I think this 1 pixel width limit could be fixed by adding some interactivity (like zooming).

### These visualizations are not a 100% representation of color
Issues with represneting colors in data
Issues with represneting colors when taking a photo
Averaging/compression can dampen colors
Down-sampling (which is what I had to do for this visualization) reduces details and can cause small shifts in colors
KMeans is still not a pure representation of colors

# Making Movies more Computable/ Computationally Tractable

# The Machine Learning Bit; Color Quanitization with K Means

# Conclusion & Next Steps

I'd like to apply a TF-IDF weighting scheme to this movie catalog - 

WHere colors (r,g,b) act as words and a "document" would be a movie. 

### What about the black bars?
The black bars actually show up as smaller frames (i.e 1920 x 1080) is really (1920 x 800) or something

### How did you source these movies?
Yargh

This way, we are able to emphasize/pickout colors that are unique to a movie, and ignoring excessively common colors (i.e (0,0,0), (255, 255, 255)) 

https://nateaff.com/2017/09/11/lego-topic-models/#:~:text=Color%20TF%2DIDF&text=In%20text%20mining%2C%20stop%20words,the%20majority%20of%20brick%20colors.

https://www.kaggle.com/nateaff/finding-lego-color-themes-with-topic-models

# Code & Stuff



# Links
https://moviebarcode.tumblr.com/
https://thecolorsofmotion.com/
https://kottke.org/19/02/movie-color-palettes
https://www.reddit.com/r/dataisbeautiful/comments/gc4mbi/oc_visualizations_of_movies_in_one_figure/
https://www.reddit.com/r/dataisbeautiful/comments/d6l2d0/oc_the_average_color_of_every_10th_frame_of_the/
https://www.reddit.com/r/dataisbeautiful/comments/d7nw9p/oc_blade_runner_2049_represented_by_1600_captures/
https://www.reddit.com/r/dataisbeautiful/comments/fmdhhr/oc_the_sleek_colors_of_joker_by_todd_phillips_one/
https://zerowidthjoiner.net/movie-barcode-generator

https://www.reddit.com/r/dataisbeautiful/comments/d7nw9p/oc_blade_runner_2049_represented_by_1600_captures/

https://www.reddit.com/r/dataisbeautiful/comments/d8ue6x/inspired_by_recent_blade_runner_barcode_both/

https://github.com/jyotiska/movie-barcode/blob/master/movie_barcode.py

# ML Links
https://scikit-learn.org/stable/auto_examples/cluster/plot_color_quantization.html#:~:text=In%20the%20image%20processing%20literature,example%2C%20uses%20such%20a%20palette.

https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html

http://charlesleifer.com/blog/using-python-and-k-means-to-find-the-dominant-colors-in-images/

https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_ml/py_kmeans/py_kmeans_opencv/py_kmeans_opencv.html

https://www.pyimagesearch.com/2014/05/26/opencv-python-k-means-color-clustering/

# Footnotes