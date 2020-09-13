---
layout: post
title: Rating Ratings
subtitle: Reviewing how people rate media &#x1F440;
published: false 
enable_latex: false
permalink: rating_ratings
frontpage: false
technical: false
funstuff: true
tags:
  - metrics
  - movie
  - review
  - stats
---

# Introduction / Why Ratings Exist 

When we think of a "rating", we tend to think of some metric that represents the quality of something like a movie, song, or game. 

The problem with ratings describing media is that we automatically imagine them as "objective" because we're used to metrics describing objective measures (weight, volume, speed, etc).

Naturally, you might think of "movie" ratings as a metric for the quality of the movie. 

This might seem fine superficially, except that when we try to rate somehting as abstract as "quality of media", we end up describing the **subjective** opinion on a piece of content rather on some objective attribute (that can be verified by everyone).

This definition might seem fitting at first, but the problem is that the "quality" of the movie is subjective. You can't measure quality of a movie when a ton of people have different opinions on how to do that. 

In reality, ratings are not metrics, but a way of communicating information about the quality of the movie.

The reason why I bring this up is because no one wants to read a 20 page article discussing the merits and flaws of "The Emoji Movie".


A lot of people will have different ways of communicating their "rating" - whethere it's a letter grade, star rating, number rating. 

# My Tier List of Statistical ratings

**Include Table of it**

S - 5 Star, Recommend/Don't Recommend
A - Tier Lists, 
B - 
C - 
D - 
F - X/Y ratings that are greater than 10 

### The ways ratings are used

"Should I watch"
"Opinion Validation" - Maybe other people don't do this (but I certainly have).
"Dick Measuring"

I like to categorize reviews in two buckets: One dimensional metrics, and multi-dimensional metrics. 

### How we understand measurement 

https://stattrek.com/statistics/measurement-scales.aspx
https://towardsdatascience.com/data-types-in-statistics-347e152e8bee

Steven's Four Scales: https://psychology.okstate.edu/faculty/jgrice/psyc3214/Stevens_FourScales_1946.pdf

Nominal - Determination of Equality
Ordinal - Determination of greater or less
Interval - Determination of equality of intervals or differences
Ratio - Determination of equality of ratios (subset of interval, but has an additional caveat that RATIO 0 is a meaningful attribute)

##### "Permissible Statistics"
Nominal - Number of cases, Mode, contingency correlation
Ordinal - Median, Percentiles
Interval - Mean, STD/VAR, Rank-order correlation, Product-moment correlation
Ratio - Coefficient of variation

# One Dimensional Metrics
These measures capture the whole experience of watching a movie script, acting, cinematography, directing into a one-dimensional 

### The Best: 5 star ratings
Much clearly - provides the ability to compare 3 star vs 4 start
Furthermore, half values (3.5, 2.5)

CON - Still doesn't provide good intro comparison
Although this doesn't apply to just 5 star reviews or movies, but faces a weird skew. 

This XKCD (although it's for amazon product reviews) demonstrates potential falls with aggregate reviews
{% assign xkcd_caption = "XKCD demonstrating how we really understand reviews"%}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/star_ratings.png" alt="fig2" caption=xkcd_caption%}


Ordinal - While restaurant ratings have a number to them, the differences between numbers don’t have a
meaning or consistency. For example, the difference between a 1 and 2 star restaurant is very different
from a 4 and 5 star restaurant, similarly the gap between a 2 and 4 star restaurant differs from a 3 start 5
star restaurant gap

### The Worst: Ratings (/100)
Terrible. There's nothing more I hate than multiplying. Just because there are more numbers/values, doesn't mean that there is increased granularity. 

Provides an unrealizable amount of precision between different movies (i.e this movie X is 2 points better than movie Y).  
Provides an (almost) arbitrary/inconsistent distinction between people.

Out of "100" scores really only work for aggregate reviews (which to be fair is how they occur).

Very sensitive to "review bombing"

##### Second to the top: Tier Lists


### Honorable Mention: Like vs Dislike
I remember the old times, when youtube was **new** and it had a 5 star rating instead of it's current "like vs dislike". These were awful times. 


PRO: Clear - you either like or dislike it

CON: Not very distginuishing, doesn't provide intra-good/bad contra disctions
ex: you don't like the same movies in the same ways. 

### SOSO: Recommend and Not Recommend
Overwhelmingly Positive
Very Postive
Positive (few reviews)
Most Positive
Mixed 
Mostly Negative
Negative (few reviews)
Very Negative
Overwhelmingly Negative

### MEH: Letter Grades
I actually think letter grades reflect

Just like our education system, media reviews 

### Bad: Cinema Sins
Way of tallying how "bad" a movie is
These tend to be problematic because you just end up watching the movie in the process.
In addition, these movies tend to be overly critical and cringey. 

To be fair I watched these cinema sins a lot! And these are not mean to be a serious critique on movies, but they're lacking and provide a distortion about the actual quality of the movie. 

# Multi-dimensional Metrics
Another under-rated factor is the quality of multi-dimensional metrics. They communicate more infromation, but they're also vulnerable to subjectivity on even **more** degrees of freedom. 


PRO: Gives the audience the ability to understand the quality of a movie on different dimensions, and allows the audience to pick what they want

### Rating the attributes of how well it succeeds in the genre movie

### Rating Acting/Cinematograpghy/Directing/Color
More nuanced, but also more work. 

Most of the time I feel like someone would be confused, and evaluate a movie to 

{% assign xkcd_caption2 = "XKCD demonstrating a two dimensional thing" %}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/fuck_grapefruit.png" alt="xkcd2" caption=xkcd_caption2}

{% assign xkcd_caption3 = "XKCD demonstrating even MORE things" %}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/so_bad_its_worse.png" alt="xkcd3" caption=xkcd_caption3}

### Actual Quality vs Enjoyment
A youtuber described this as his 
I think this was a great way of capturing the q

https://www.youtube.com/watch?v=9C_9SH5BgUc

Clean enjoyable way 

it seperates the quality of the movie (i.e something to satisfy critics) with how it is to actually watch the movie (i.e normal people)

There are some high quality movies that I don't enjoy, but I appreciate. 

There are some low quality movies that I really enjoy. 


### Watching Alone/Watching Together

Watching Together part is a good metric for measuring how easy it is to watch with freinds and have banter - similar to the "Ironic enjoyment metric"



### Critic Enjoyment, Audience Enjoyment 

I actually 

I enjoy it because I can guess movies that might appeal to hardcore movie enthusiasts (read snobs) but also regular movie watchers


### Conclusion
In conclusion, picking the right metric depends on who you're trying to communicate to.

If you're trying to communicate your complex nuance about the complicated nuance of a piece of media, you shouldn't picking a one dimensional metrics 