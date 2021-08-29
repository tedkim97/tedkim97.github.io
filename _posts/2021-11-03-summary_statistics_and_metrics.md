---
layout: post
title: Practical Statistics for Software Engineering - Descriptive Statistics
subtitle: Latency Study with Tangents
published: false
enable_latex: false
enable_d3: false
permalink: pract_stat_2
frontpage: true
technical: true
funstuff: false
tags:
  - programming
  - communication
  - statistics
  - software
  - metrics
  - case-study
  - guide
concepts:
  - statistics
  - metrics
  - software engineering
curated:
  - pract_stat_1
  - gaitkeep
  - better_barcode_vis
---

# Introduction 

Continuing with statistics for software engineering - I thought I would continue with a summary of descriptive statistics and some tangents like "metrics and (TODO) measurability.

My goals isn't to list various summary statistics and their formulas. My goal is to demonstrate the nuance or details necessary for understanding the decision making of picking the correct summary statistics, and the broader practice of describing data. 

# STATISTIC (NOT TO BE CONFUSED WITH STATISTICS)

A statistic (singular) or sample statistic is any quantity computed from values in a sample which is considered for a statistical purpose. Statistical purposes include estimating a population parameter, describing a sample, or evaluating a hypothesis. The average (aka mean) of sample values is a statistic. The term statistic is used both for the function and for the value of the function on a given sample. When a statistic is being used for a specific purpose, it may be referred to by a name indicating its purpose.

When a statistic is used for estimating a population parameter, the statistic is called an estimator. A population parameter is any characteristic of a population under study, but when it is not feasible to directly measure the value of a population parameter, statistical methods are used to infer the likely value of the parameter on the basis of a statistic computed from a sample taken from the population. For example, the mean of a sample is an unbiased estimator of the population mean. This means that the expected value of the sample mean equals the true mean of the population.[1]

In descriptive statistics, a descriptive statistic is used to describe the sample data in some useful way. In statistical hypothesis testing, a test statistic is used to test a hypothesis. Note that a single statistic can be used for multiple purposes – for example the sample mean can be used to estimate the population mean, to describe a sample data set, or to test a hypothesis.

# Descriptive Statistics 
Descriptive statistics is the "art of summarizing data" - meaning that the point of descriptive statistics is to communicate information about your data as simply as possible. 

Descriptive statistics is often contrasted with "Statistical inference" which focuses on valid generalizations from the data collected - essentially taking a small portion of possible data (population) and generating conclusions about the population.

This contrast is important because inference involves quantifying uncertainity when estimating the population, relying on probability and models to come to the conclusion. 

On the otherhand, descriptive statistics focuses on accurately summarizing the data.



A table of Descriptive statistics often comes with several summary statistics 

The OpenIntro Statistics book describes a **summary statistic** as "**a single number summarizing a large amount of data**" 

"Open Intro": A summary statistic is a single number summarizing a large amount of data. (pg 10, 4th edition) 

"Statistics": Descriptive statistics — the art of summarizing data — is (Preface xiv, 4th edition)

Let's be real though - we're probably going to rely on the wikpedia definition anyways

According to wikipedia -

"Descriptive statistics "
and "Summary Statistics"

https://en.wikipedia.org/wiki/Descriptive_statistics
https://en.wikipedia.org/wiki/Summary_statistics

## Why do we need descriptive statistics?
Inference is the main part of statistics people "care" about - often times people don't care about  

While there are overlaps between summary statistics in both descriptive and inference statistics (variance/std, mean, median, etc), descriptive statistics arne't used for inference.  


So why should we care about descriptive statistics?

1. Brevity (They are fast & comfortable summarizes) 
- Is important for summarizing data - most people won't have the time to comb through the data as you did.

Instead of looking at all your logs or visualizing the distribution, you can just have a 1-2 descriptive statistics to get an idea about the shape of your service. 

(EXAMPLES HERE)

but it's a balancing act.

One or two descriptive statistic will rarely communicate a complete picture about the series, but it will provide context 

2. Context
- They don't have intuition for your domain, as a result summary statistics are a great way of establishing context
- Anomly detection & stuff

3. They guide and inform decisions about what kind of tests we should use and run 
- The standard deviation of our data is so large, any kind of inference we draw from it would be useless 
- In the previous blog post (TODO: LINK) descriptive statisitcs along with our data visualization of our latency distributions informed us of the types of distributions we should use to model our inference. 


## What are some examples of Summary Statistics?

Fall into categories 

### Numbers

average(s); correlation; interquartile range; measurement
error; median; percentiles;
regression effect; r.m.s.;
SD; slope"

### Visualizations

Visualizations are also descriptive statistics as well - they are meant to summarize the data you are measuring - all of the distributions in the previous examples were summary statistics, with more summary statistics imposed on them!

scatter plots, histograms for data;  normal curve;

Scatter plots are another kind of 

# The Case Study

Using our newfound knowledge of descriprive staitstics, which numbers should we use?

## Let's say you need some dashboard in Datadog, New Relic, Splunk, etc

## Measuring how well the data fits does this the statistic fit  Is this an accurate number? (It Depends)
Depenidng on what our data looks like the "correct" summary statistic you use could differ. 

If our data looks something like this:

{% assign caption1 = "Sample histogram generated with numpy & matplotlib" %}
{% include caption_image.html imgpath="/figures/distribution_figs/measuring_latency_simple_histogram.png" alt="small example of histogram of latencies" caption=caption1 %}

or this

Completely flat distribbutions (although there is probably something seriously wrong if your data)

then using mean and standard deviation are perfectly fine.

## Dealing with Skew

But what happens if your data looks like this?
**Incredibly skewed distribution**
Very clearly most of your data occurs between X and Y ms, but because of your outliers the average is very unrepresentative of your day.
- As a result you might pick median results or percentiles to characterize your data

### Tangent: Income statistics
Income is one of the most popular examples of choosing median income vs average income.


In this graph provided by the [federal reserve](https://fred.stlouisfed.org/series/MEPAINUSA672N#0) we see that mean income is almost $20,000 in the late 2020's rather than the median income, why is that?

<iframe src="https://fred.stlouisfed.org/graph/graph-landing.php?g=GjGm&width=670&height=475" scrolling="no" frameborder="0" style="overflow:hidden; width:670px; height:525px;" allowTransparency="true" loading="lazy"></iframe>

The reason is how these numbers are calculated. The median takes the value at the 50% - which is unaffected by the actual dollar amount each household earns.

However, the average accounts for the dollar earned. When individuals earn an extraordinary small or large amount of money, they can drag the number down or up.


# Tangent: Be careful about what you are **really** measuring

## Path lengths can affect code execution (and other seemingly trivial things)

https://www.reddit.com/r/programming/comments/ozytw8/why_two_identical_methods_run_at_different_times/h86klch/

https://twitter.com/badamczewski01/status/1423683244264988672?s=12
- This isn't a problem specific to rust or .NET as well. I saw a conference talk on proposed tool for measuring execution time of programs by varying path lengths. The presenter gave a background how they discovered path lengths could lead to different execution times depending on the machine it was run on.

Are you measuring the implementation of an algorithm, or are you measuring the performance of an algorithm after optimized by compilation, or with a hardware accelerator, etc. 

Here's a story that you might here. 

Ah here's the benchmark on the remote server, and here's my new feature promising 100x speedup on my local branch. I've benchmarked with several runs (n= 1000000) and I've also varied the inputs.
- I've shown the minimum, maximum, median, average, std, IQR runtimes, and it shows that I have 10x speed up with the same amount of std

- Is this correct?

## SonarQube Metrics

## Averaging Color

I made a whole blog post "critique" about someone using average colors and interpolated frames to represent color usage in movies, but that just isn't realistic

# Tangent: Metrics

Since this is a software context - something we need to mention are "metrics"

Metrics (by themselves) don't really involve any statistics. There might be statistics *around* metrics i.e how correlated is X metric with Y

To be honest, I don't have a formal definition for "metric" aside from a system of measurement. 


It was one of those word I picked up in my undergrad career from nonprofit consulting, and my working understanding just "worked" in other applications in my life.

My working understanding is that it is a quantitative measures meant to be used as proxies/indicators of more abstract concepts.
i.e "user engagement" might be a combination of likes, comments, shares on $SOCIAL_MEDIA_PLATFORM
i.e "velocity" in the agile framework may be quantitatvely measured as number of Jira tickets or "Story points" completed in a period of time - but is meant to represent the productivity of the team.

I don't have many favorable opinions of "metrics"

## Some metrics feel completely meaningless/misleading

## Often time there's an audible groan whenever "metrics comess up"

## Metrics tend to degrade over time
https://en.wikipedia.org/wiki/Goodhart%27s_law

https://www.agilealliance.org/glossary/velocity/#q=~(infinite~false~filters~(postType~(~'page~'post~'aa_book~'aa_event_session~'aa_experience_report~'aa_glossary~'aa_research_paper~'aa_video)~tags~(~'velocity))~searchTerm~'~sort~false~sortDirection~'asc~page~1)

https://www.planview.com/resources/articles/lkdc-velocity-agile/#:~:text=Velocity%20in%20Agile%20is%20a,completed%20in%20a%20given%20timeframe.&text=Once%20this%20is%20measured%20based,plan%20to%20complete%20per%20sprint.



# Visualizations don't hurt

# Conclusion

# Citations
Diez, David M., Christopher D. Barr, and Çetinkaya-Rundel Mine. “Introduction To Data”. In OpenIntro Statistics. OpenIntro, Inc., 2019.

Statistics book
Open Intro 
Wikipedia <3

https://stats.stackexchange.com/questions/249015/what-is-the-point-of-reporting-descriptive-statistics/249021

https://stats.stackexchange.com/questions/290648/meaning-of-metric-vs-statistic-vs-parameter

https://stats.stackexchange.com/questions/47728/what-is-the-difference-between-an-estimator-and-a-statistic

# Footnotes