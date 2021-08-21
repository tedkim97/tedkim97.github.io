---
layout: post
title: Practical Statistics for SoftwareEngineering - Descriptive Statistics
subtitle: Latency Study Continued with tangents
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
curate:
  - pract_stat_1
  - gaitkeep
  - better_barcode_vis
---

# Introduction 

Continuing with statistical lessons for software engineering - I thought I would continue with a discussion of descriptive statistics, "metrics", and a tangent about measuring software.



We're profiling the performance of our code, users, services,

The code we commit are valued and rejected based on "metrics".

While I could just repeat formulas like means, medians, standard deviations, IQRs, 

# Statistic (not to be confused with Statistics)

A statistic (singular) or sample statistic is any quantity computed from values in a sample which is considered for a statistical purpose. Statistical purposes include estimating a population parameter, describing a sample, or evaluating a hypothesis. The average (aka mean) of sample values is a statistic. The term statistic is used both for the function and for the value of the function on a given sample. When a statistic is being used for a specific purpose, it may be referred to by a name indicating its purpose.

When a statistic is used for estimating a population parameter, the statistic is called an estimator. A population parameter is any characteristic of a population under study, but when it is not feasible to directly measure the value of a population parameter, statistical methods are used to infer the likely value of the parameter on the basis of a statistic computed from a sample taken from the population. For example, the mean of a sample is an unbiased estimator of the population mean. This means that the expected value of the sample mean equals the true mean of the population.[1]

In descriptive statistics, a descriptive statistic is used to describe the sample data in some useful way. In statistical hypothesis testing, a test statistic is used to test a hypothesis. Note that a single statistic can be used for multiple purposes – for example the sample mean can be used to estimate the population mean, to describe a sample data set, or to test a hypothesis.

# Descriptive & Summary Statistics 

According to wikipedia -

"Descriptive statistics "
and "Summary Statistics"


In terms of semantics - 

## What's the point of descriptive statistics

The point of descriptive statistics is to communicate information about your data as simply as possible. 

Instead of looking at all your logs or visualizing the distribution, you can just have a 1-2 descriptive statistics to get an idea about the shape of your service. 

(EXAMPLES HERE)

but it's a balancing act.

 One descriptive statistic will rarely communicate a whole picture about the series. 


## N-Number Summaries


## Let's say you need some dashboard in Datadog, New Relic, Splunk, etc

## does this the statistic fit  Is this an accurate number? (It Depends)

# Tangent: What are you **really** measuring

## Path lengths can affect code execution (and other seemingly trivial things)

https://www.reddit.com/r/programming/comments/ozytw8/why_two_identical_methods_run_at_different_times/h86klch/

https://twitter.com/badamczewski01/status/1423683244264988672?s=12
- This isn't a problem specific to rust or .NET as well. I saw a presentation on proposed tool for measuring execution time of programs by varying path lengths. The presenter gave a background how they discovered path lengths could lead to different execution times depending on the machine it was run on.



Are you measuring the implementation of an algorithm, or are you measuring the performance of an algorithm on your machine, compilation benefits, etc. 

Here's a story that you might here. 

Ah here's the benchmark on the remote server, and here's my new feature promising 100x speedup on my local branch. I've benchmarked with several runs (n= 1000000) and I've also varied the inputs.
- I've shown the minimum, maximum, median, average, std, IQR runtimes, and it shows that I have 10x speed up with the same amount of std

- Is this correct?

## SonarQube Metrics

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

# Conclusion