---
layout: post
title: Why I Try Link As Much As Possible 
subtitle: Citations Needed
published: true
enable_latex: false
enable_d3: false
permalink: link_more
frontpage: false
technical: false
funstuff: true
tags:
  - programming
  - communication
  - citations
  - blog
  - software
  - learning
  - casual
  - essay 
concepts:
  - opinion
  - meta-blog
  - casual
---

# Background & Opinion

After reviewing the posts here I've realized that I'm a bit excessive with linking definitions, evidence, documentation, or external sites. I say this because it makes the reading experience feel distracting, and it deviates from other blogs - which tend to be more sparing with their linking. 

But at the risk of being harpooned for being an idiot, I'm going to state my opinion on the internet. Mainly I think that digital content should link and cite information more rigorously, something like the citation requirements in academia. 

> While this is unrealistic for most blogs that use personal/professional experience, these blogs can at least be explicit about knowledge gained from papers/online sources vs knowledge passed down from teams/experience. 

# My Argument

A part of my reasoning is fairness - if one benefited from other's contributions, it's important to acknowledge their contribution. However, the main reason for linking is because we need better information tracing because the web (as a resource) has ***lots*** of flaws.


## You Can Publish (Almost) Anything On The Internet

Anyone can publish on the internet[^1]. With website creation tools, hosting services, affordable domain names it's much easier to publish lazy/untrue/slanderous content on the "mainstream" web (as opposed to academic journals and newspaper)[^2]. 

[^1]: I'm pretty sure I don't need a citation on this...
[^2]: I'm pretty sure I do need a citation on this

A more horrific extension is when corporations publish content that is effectively propaganda on the web. Notable examples include the "amazon post" and "judgeforyourselves". 

Amazon Post is a Chevron sponsored "news" site dedicated to discrediting their target [Steven Donziger](https://en.wikipedia.org/wiki/Steven_Donziger), the human rights attorney who won in a case against them[^3]. The site literally says "Chevron's Views And Opinions On The Ecuador Lawsuit". 

For those that can't remember Ecudator, Judgeforyourselves is a legal propaganda attempting to divert blame regarding the Purdue Pharma, Sackler family, and the opioid crisis.

[^3]: Overall the situation surrounding Steven Donziger is depressing. While there is evidence that Donziger's prosecution against Chevron had ethical flaws, [the lawyer's disbarment in addition to 2 ongoing years of house arrest are examples of judicial harassment](https://youtu.be/0wxaIqc8o6s). Some other readings: [the intercept article](https://theintercept.com/2020/01/29/chevron-ecuador-lawsuit-steven-donziger/) and [vice](https://www.vice.com/en/article/neye7z/chevrons-star-witness-admits-to-lying-in-the-amazon-pollution-case). 

Without linking, an individuals might regurgitate information from these sources without having the opportunity for an individual to point out the flaws in their sources. 

> With the recent case of [AWS](https://www.bloomberg.com/news/articles/2021-01-11/amazon-s-removal-of-parler-shows-cloud-unit-s-rarely-used-power) and [other cloud providers](https://www.businessinsider.com/parler-offline-hosting-ceo-john-matze-amazon-large-website-providers-2021-1) rejecting social media app Parler , there is an interesting precedent for companies boycotting certain content on the mainstream web. 

## Search Engines Are Trusted... But Imperfect
For context, I grew up viewing "the internet" as an auxiliary parent. I could ask Google for help on my homework, and find the answers to questions like "how to convert Celsius to Fahrenheit", "area of a triangle", or *super embarrassing questions* I was too uncomfortable asking other people. 

I don't think this is a uncommon notion either, as this [survey on statista (I don't have premium statista, so I can't review the source)](https://www.statista.com/statistics/381455/most-trusted-sources-of-news-and-info-worldwide/) show that search engines are the more trusted source. The reason why I'm emphasizing this is because google can show questionable results especially when it [comes to opinions](https://www.wsj.com/articles/googles-featured-answers-aim-to-distill-truthbut-often-get-it-wrong-1510847867) or [advertisements](https://www.nytimes.com/2020/01/31/technology/google-search-results.html).

From my personal experiences, it seems that google search results are becoming increasingly polluted with garbage. Medium articles and Youtube videos have become incredibly more common as "top results" for technical searches. 

It might be because my cookies indicate that I click these links, so I get them recommended more, or because other people click these links and use them as helpful resources. In the end, most of these resources are poor alternatives to actual documentation, papers, wikipedia, etc. 

Query filters can easily fix this problem (which I eventually had to do), but the most vulnerable group (inexperienced users) have to sort or consume the junk. 

## Everything on the Internet points to each other (but with almost no citations)

Internet content ends up copying content from textbooks, books, or other internet content. 

An infamous example is Siraj Ravel who's earliest (and current) "tutorials" plagiarized of other people's repositories, official documentation, and academic papers[^4]. An example I found was his ["CUDA Tutorial"](https://www.youtube.com/watch?v=1cHx1baKqq0) copying the contents of [Nvidia's own "An Even Easier Introduction To CUDA"](https://developer.nvidia.com/blog/even-easier-introduction-cuda/)

[^4]: There is a ton of videos and articles covering this drama - here is a "unbiased" source: https://www.theregister.com/2019/10/14/ravel_ai_youtube/ but I recommend searching yourself.

A worse version of this is when content points to each other with poor citations - leading to propagation of unverifiable information. For instance, the commonly cited fact "bugs found in requirements are 100x cheaper than bugs found in implementations" is unverified and probably made up. [This blogpost describes the wayne's adventure discovering this information and the struggles of academia in empirical software research - I recommend reading it](https://buttondown.email/hillelwayne/archive/i-ing-hate-science/).

A non programming example that I was personally upset about is when [I made custom chocolate bars]({% link _posts/2019-8-29-custom_chocolate_mold.md %}). I received the inspiration from a series on making chocolate from (and a lot of the instructions from) the [youtuber "Alex"](https://www.youtube.com/user/FrenchGuyCooking). However, after searching a bit more on tempering chocolate (a process of heating and cooling chocolate to reach an ideal texture), I've discovered that the youtuber's methods (and information presented) are very similar to [information found on J. Kenji López-Alt's guide on chocolate](https://www.seriouseats.com/2014/12/the-food-lab-best-way-to-temper-chocolate.html). It doesn't dampen any of the creativity or story-telling of the video, but it's slightly disingenuous to present these innovative discoveries without proper credit. 

## Linking = Easier Verification

It's possible for conventional wisdom to be wrong about a subject, and for the general public to take a while to catch up with the more "nuanced" wisdom[^5]. What's wonderful is that normal people can verify (or at least be convinced) that conventional wisdom is wrong with evidence (Science!). However, if you don't provide a way to locate this evidence, the burden of verifying these claims is on the burden of the learner. This just ends up being an incredibly frustrating and confusing experience for the interested.

[^5]: I can imagine anecdotal evidences, but no citations with systematic studies of "conventional knowledge vs reality"

In this ["best practices" post](https://newrelic.com/blog/best-practices/expected-distributions-website-response-times) by NewRelic, a company that provides monitoring services and apis, the article states that one of their engineers knows that latency distributions more follow an erlang distribution rather than a log-normal distribution. Then there is absolutely no citation, evidence, or reasoning, so I'm left furiously researching (and trying to find results)[^6]. On the other hand, the same post links research detailing reasoning and methodology for why log normal distributions generally fit latency measurements (Ciemiewicz, David M. "What Do You mean?-Revisiting Statistics for Web Response Time Measurements." In Int. CMG Conference, pp. 385-396. 2001) - where I can *pretend* to understand the research's and share it. 

[^6]: To be fair, the individual probably was pestered into contributing to this and had other priorities

The engineer in question could be right - but without a link or clue I found it incredibly difficult to verify these claims. As a result, I was just annoyed, confused, and unconvinced. 

## "Common Knowledge" doesn't exist on the internet
From my experience, people (even I) tend to overestimate what is "common knowledge". 

<img src="https://imgs.xkcd.com/comics/average_familiarity.png" loading="lazy" alt="xkcd 2501" class="center-image"/> 

Often times people exclude citations because the author believes this information is "common knowledge" - meaning that the audience has a shared body of knowledge that they all understand, such as the definition of "validation error" or how to quickly find prime factors. 

Common knowledge exists for academia - where the intended audience is experts. However, the web is a very broad audience. A beginner/intermediate programmer can be interested in what you do, and while this individual might understand concepts like classes, encryption, language paradigms they might not be familiar with other fundamentals topics like compilers, DNS, architecture, etc. 

While you could explain the foundational topics relevant to your post, that's a lot of effort for relatively little payoff. On the positive side, linking provides the opportunity for curating learning resources, increasing long-term accessibility to your subject, and accelerating their learning rather than just having them awkwardly accrue knowledge. 

## People Believe and Repeat Incorrect Information 

Another reason I emphasize citations is because I've had several "hmmmmmm" moments throughout my life. I would hear things that were incredibly dated, ignored context, or just incorrect.

Some of them were silly, like ["if you close the doors with an open window or door you will die"](https://en.wikipedia.org/wiki/Fan_death). Some of them were more serious where people (who I respect, have respectable jobs, etc) might cite flawed statistics, faulty science, or lies. When I asked the individual for the source of that information (or quick google results), they would have a difficult time actually finding this information. 

> I don't expect people to carry a bibliography for every bit of knowledge they have, but if one uses "science" to back a larger argument the least you can do is show *some* source.  

> Some examples I've heard with empirical evidence/expert testimony demonstrating otherwise: ["undocumented immigrants take more resources from the government resources than they contribute"](https://www.pbs.org/newshour/economy/making-sense/4-myths-about-how-immigrants-affect-the-u-s-economy), ["the Laffer Curve says lowering taxes will actually increase tax revenue"](https://www.washingtonpost.com/politics/2019/06/01/trump-is-giving-arthur-laffer-presidential-medal-freedom-economists-arent-laughing/)

My experiences and the global attention on fake news and disinformation in the 21st century is just a reminder of the severity of this problem,  and demonstrates the need to verify facts we hear. 

Linking provides accountability to help combat this. That way when a tweet or blog links "scientific study: `some_antivax_blog.net`", people can judge the reliability of the info[^7]. 

[^7]: But to be honest of my own biases, I'm a a bit more lax about verifying this information if I already believed it

# Weaknesses

I've spent most of this post arguing for more aggressive linking and citation patterns on the internet, but I know that's not always viable or the goal. 

For instance, a lot of details are not cited in this post because I can't spend more months researching and drafting a 10 minute read. If everyone followed a verified citation pattern, everything would just become Wikipedia (I wish) - but more importantly the work needed to maintain this pattern isn't realistic for most people. 

Furthermore, more citations aren't correlated with higher quality information. Plenty of peer-reviewed, published papers end up being critiqued or debunked in the future (but that's the beauty of the scientific process).

Another flaw is that links disappear - a website you linked might exist for the next 5-10 years, but might not exist for the next 20 or 50. 

# Ending Note

This was my reasoning (and opinion) for why we should pursue more rigorous citation styles with digital content. I always welcome discourse so reach out to me through email: `nablags97 [at] gmail [dot] com`.

