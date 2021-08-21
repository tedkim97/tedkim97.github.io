---
layout: post
title: Rating Ratings
subtitle: Reviewing how people rate things
published: false 
enable_latex: false
permalink: rating_ratings
frontpage: true
technical: false
funstuff: true
tags:
  - vis
  - metrics
  - movie
  - review
---

# TLDR
1. The current rating systems is awful
2. Ratings seem more like a communication system rather than a measurement one, and as a result we should treat them as such
3. I don't provide any alternatives, but I rate some other 

# Introduction 

I think the rating systems (&#9733;&#9733;&#9733;, 100%, 10/10, etc system) we use are mediocre. They aren't *terrible* and popular for a reason, but I think it's time to evaluate the alternatives we have for rating media.

This post is a "for-fun", conversational, opinion piece, rather than a academic dissection - but I think there's value in re-examining the metrics people use to evaluate media. 

# What do we mean by ratings & how do we used them?

The dictionary defines "rating" as a classification relating to rank or grade. When people think of a "rating", people think of a letter grade (A, B, C) or star system (1&#9733;, 2&#9733;, 5&#9733;, etc) that represents the quality of a thing (performance, build, customer service, etc). Naturally, ratings are seen as "objective" through their association with measurable qualities such as performance, form factor, build quality, customer service, etc.

> This glove is very soft and very warm! Therefore it has a 5/5 rating.
> This glove is also soft, but not very warm so it has a 2/5 rating.
> This glove is warm, but very rough so it has a 3/5 rating.

### The Misunderstanding:
The problem with ratings is that we're trying to describe fuzzy ideas like "quality" when it comes to artistic endeavors in media (like movies, songs, art, or games). You could describe a high quality film with its production value - using visuals, soundtrack, or acting as indicators of quality. However, there are too many factors (like novelty, themes, pacing) when describing these pieces that make describing "quality" difficult.

There are handful of examples of movies with great production quality (i.e big budget) that fail as art pieces! Some examples that critics/audiences have examined are [Movie 43](https://en.wikipedia.org/wiki/Movie_43) (a comedy with cast with over 30 star celebrities that is considered one of the worst movies of all time) or [Transformers: The Last Knight](https://www.imdb.com/title/tt3371366/?ref_=tt_sims_tt) (a profitable Transformers movie, but wasn't very engaging overall). In addition, there are some high-budget games that meet this criteria as well. [Anthem](https://en.wikipedia.org/wiki/Anthem_(video_game)) was a very hyped video game, with high quality production but mediocre gameplay that almost killed the game. I can't think of anything in others music or art (except [Spider-Man: Turn Off the Dark](https://en.wikipedia.org/wiki/Spider-Man:_Turn_Off_the_Dark) and theater!).
 
My issue with ratings is that we see a number and assume it's a metric for the "quality" of media. A movie rating *seems* like it should measure the quality of a movie. However, I argue that ratings tend to reflect how much the individual enjoyed the movie. As a result, I think that media-ratings are rarely a measurement for objective quality ("the cinematography and acting were stunning"), but subjective quality - mainly measuring personal enjoyment ("I had fun watching the movie. It was pretty, and engaging").

##### Ratings are communication - not measurement
This is a small but important distinction because the new phrasing emphasizes the **communication of opinion** rather than description of quality. For some people, quality might play into their opinion, but the idea of "quality" in the arts is also subjective.  

These two ideas sound the same (and you might already have this idea about ratings), but I think establishing subjectivity removes the illusion of rigor because it uses numbers and decimals. Understanding ratings as communication rather than a measurement acknowledges the subjectivity of it. 

# With this understanding of ratings, I think we should rate our ratings with this new understanding

# My Rating of Statistical ratings
I like to categorize ratings in two buckets: One dimensional metrics, and multi-dimensional metrics. 

**Include Table of it**

S - Recommend/Don't Recommend
A - Tier Lists, 
B - 
C - 5 Star
D - 
F - X/Y ratings that are greater than 10 


# Understanding the "dimensions" of ratings (single vs multi dimensional)
I like to categorize ratings as one dimensional or multi-dimensional - which can be analogous to a scalar vs a vector in linear algebra. A single rating for a movie might be a `4/5` - while a multidimensional rating might be a vector corresponding to `<cinematography, soundtrack, acting>` and look like `<3/5, 2/5, 4/5>`. (To appease the Linear Algebra purists) it's important to note that even though my description *seems* like a vector, the ordinality of the scores prevent it from having consistent vector operations. 

### Single Metric
- The main pro of a single dimensional rating is that it captures the whole experience and reduces it into a single number. 
- The fastest way of understanding the quality of something.

The main con is that single-number ratings make anonymous communication confusing. If you have a friend or reviewer that you consistently get recommendations for, you can build an idea of what they attributes value in a piece of media.

A stranger can give you a rating that may be too high or too low because of differences in value. A stranger might care about the soundtrack or acting of a movie, while you only care about fight sequences. As a result, single numbers result in an unclear communication of values. 


### Multi-dimensional
I think multi-dimensional ratings (ratings that communicate multiple facets of pieces of media) provide more clarity to what contributes to a "quality" movie and allows the audience to pick which attributes they care about.

However, these metrics aren't more objective or accurate than a single dimensional metric. 

In fact, even though they communicate more information - they're vulnerable to subjectivity on even more degrees of freedom.  

Another potential downfall is that it forces people to examine aspects of media they aren't invested in or care about.

They just make it easier to discern **why** someone thicks a movie is bad or good.

# One Dimensional Metrics
### The "Best" of the Worst: 5 &#9733;/star/\* ratings

##### The Pros:
- The 5 star rating is one of fastest ways to understand something. 
- The star rating provides a clear distinction between the lowest and highest values. A 1\* movie is the *worst* movie, and a 5\* movie is the *best* movie. 
- The star rating provides a clear hierarchy of star ratings. There's a clear distinction between a 3\*; and 4\*; movie
- Furthermore, half values (3.5\*, 2.5\*) provide some granularity, without providing arbitrary in-betweens (2.1\*, 3.02\*, 4.0005\*).

##### The Cons:
The cons I described here are mostly based off the typology described in [Stevens, S. S. "On the Theory of Scales of Measurement." Science 103, no. 2684 (1946): 677-80](https://www.jstor.org/stable/1671815?origin=JSTOR-pdf&seq=1). I was taught about these scales of measurements (nominal, ordinal, interval, ratio, etc) in my (intro) statistics classes and included in several textbooks; however, it's important to note that the work has legitimate critique against it. 

- Star ratings are [**ordinal**](https://en.wikipedia.org/wiki/Ordinal_data). Meaning that even though ratings have a number, the differences between these numbers don't have meaning or consistency. For example, the difference between a 1\* and 2\* restaurant is very different from a 4\* and 5\* restaurant. A gap between the 2\* and 4\* restaurant differs from the 3\* and 5\* restaurant gap[^1].

[^1]: An example of data types where the difference between the values matter are **Interval** and **Ratio**. For instance, an example of something with meaningful difference is Mass. The difference between a 12kg and 10kg object is the same as a 5kg object and 3kg object. 

- The most common way of aggregating this review data is by averaging. Unfortunately, taking the mean is not a "[**permissible statistic**](https://en.wikipedia.org/wiki/Ordinal_data#Ways_to_analyse_ordinal_data)" for ordinal type data. The reason being that operations like addition and division don't make sense for ordinal data, and the mean wouldn't make sense as well. A more appropriate summary statistic would be to use the median, mode, or percentile.

- Similar to the idea above, a lot of star ratings face a weird skew (people tend to rate things as 5\* or 1\*) and so aggregating reviews give unclear results. I've stated that product reviews are different from movie reviews, this XKCD demonstrates pitfalls with 5\* aggregate reviews
{% assign xkcd_caption = "XKCD demonstrating how we really understand reviews"%}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/star_ratings.png" alt="fig2" caption=xkcd_caption%}

### The Worst: Ratings out of 100 (X/100, 52%, etc)
This review system is the worst in every way. 

**The Pros:**
- Provides more granularity to compare the ranking of various art pieces together. 

**The Cons:**
- Provides an unrealistic amount of precision between different object (i.e song X is 2 points better than song Y).  
	- Implies that there is a level of precision that probably doesn't exist.
- Out of "100" scores really only work for aggregate reviews, but that doesn't mean reviews take advantage of this granularity. 
- Provides an arbitrary/inconsistent distinction between people

### Like vs Dislike
Before Youtube introduced the [Like/Dislike system in 2010](https://en.wikipedia.org/wiki/Like_button#YouTube), the site had a 5 star rating system. 

These were awful times.

It turns out, the majority of youtube users very rarely described things with a 1 or 5 star. They just used 5 stars.


The reason why is that youtube videos were rated on a 5 start system. It worked fine for videos that had a lot of eyeballs, but small videos would be misrepresented because someone could make 4 youtube accounts, rate their video "5 stars" - while an actual user would rate it 1 star. Now an terrible/misleading video was given a 4 star rating. 

**The Pros:**
- Clear, Distinct (no confusion)
- Cleanest and clearest message for aggregation (Like/Dislike Ratio)

**The Cons:**
- No nuance from a "Like" or "Dislike" rating. Implies you like/dislike things the same
	- For example, an individual likes both "Mad Max: Fury Road" and ""  

##### SOSO: Recommend and Not Recommend
Steam introduced a "recommend and not recommend" feature for Steam game. 

almost identical to the Like/Dislike Metrics Derived from Like/Dislike

The metric provides a distinction in that it provides a clear rating at the end "like enough to recommend" or "disliked it, wouldn't recommend". In addition, users would fill out a short or long review describing why they attributed their recommendation. 

(Instead of seeing a Like/Dislike ratio)
Afterwards, Steam would aggregate these reviews to form an "aggregate opionion" about these games that would simplify to:
- Overwhelmingly Positive
- Very Positive
- Positive (few reviews)
- Mostly Positive
- Mixed 
- Mostly Negative
- Negative (few reviews)
- Very Negative
- Overwhelmingly Negative

##### Second to the top: Tier Lists

### MEH: Letter Grades
I actually think letter grades reflect

Just like our education system, media reviews 

### META: Pro Con
Pro: let's people weigh their own pro-cons about a movie 
Con: Has the effect of blowing things out of proportion

### Bad: Cinema Sins
Way of tallying how "bad" a movie is
These tend to be problematic because you just end up watching the movie in the process.
In addition, these movies tend to be overly critical and cringey. 

To be fair I watched these cinema sins a lot! And these are not mean to be a serious critique on movies, but they're lacking and provide a distortion about the actual quality of the movie. 



# Multi-dimensional Metrics
### Rating the attributes of how well it succeeds in the genre movie

### Rating Acting/Cinematograpghy/Directing/Color
More nuanced, but also more work. 

Most of the time I feel like someone would be confused, and evaluate a movie to 

{% assign xkcd_caption2 = "XKCD demonstrating a two dimensional thing" %}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/fuck_grapefruit.png" alt="xkcd2" caption=xkcd_caption2 %}

{% assign xkcd_caption3 = "XKCD demonstrating even MORE things" %}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/so_bad_its_worse.png" alt="xkcd3" caption=xkcd_caption3 %}

### Actual Quality vs Enjoyment
A youtuber described this as his 
I think this was a great way of capturing the q

https://www.youtube.com/watch?v=9C_9SH5BgUc

Clean enjoyable way 

it seperates the quality of the movie (i.e something to satisfy critics) with how it is to actually watch the movie (i.e normal people)

There are some high quality movies that I don't enjoy, but I appreciate. 

There are some low quality movies that I really enjoy. 


### Watching Alone/Watching Together

Watching Together part is a good metric for measuring how easy it is to watch with friends and have banter - similar to the "Ironic enjoyment metric"


### Critic Enjoyment, Audience Enjoyment 

I actually 

I enjoy it because I can guess movies that might appeal to hardcore movie enthusiasts (read snobs) but also regular movie watchers


### Conclusion
In conclusion, picking the right metric depends on who you're trying to communicate to.

If you're trying to communicate your complex nuance about the complicated nuance of a piece of media, you shouldn't picking a one dimensional metrics 


# Rating Media is not fair because there is a bias with respect to media
We rate things we enjoy, unlikely (or at least for me) to watch/play/listen to something unless I know i'll like it, or hate it. 

### Vote Manipulation & Curation
I used to be a big aggregate guy, that a single reviewer might not understand the needs of lay person like me



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

Why don't people use medians in ratings? Well, I suspect that the median rating for a lot of "things" look something like this:

This XKCD (although it's for amazon product reviews) demonstrates potential falls with aggregate reviews

{% assign xkcd_caption = "Buying/Watching stuff sucks now"%}
{% include caption_image.html imgpath="https://imgs.xkcd.com/comics/reviews.png" alt="fig3" caption=xkcd_caption%}


