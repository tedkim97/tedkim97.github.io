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

After reviewing the posts here I've realized that I'm a bit excessive with linking definitions, evidence, documentation, or external sites. I say this because it makes the reading experience feel distracting and deviates from other blogs - which tend to be more sparing with their linking. 

But at the risk of being harpooned for being an idiot or not getting the point, I'm going to state my opinion on the internet. Mainly I think that digital content should link and cite information more rigorously, something like the citation requirements in academia. 

> While this is unrealistic for most blogs that relies on personal/professional experience, these sources can be explicit about distinctions from from papers/online sources vs knowledge passed down from teams.

A part of the reason for linking is just to be fair - if one benefited from other's contributions, it's important to acknowledge their contribution. However, there's a more important reason for higher demand for info traceability - mainly that the internet has lots of flaws. 

# The Argument

## You Can Say Publish (Almost) Anything On The Internet

Anyone can say anything on the internet[^1]. With website creation tools, hosting services, affordable domain names it's much easier to put anything on the "mainstream" web than on an academic journal or newspaper[^2]; however, it is harder (or pricier) to get a random website to the top of search engines. 

[^1]: I'm pretty sure I don't need a citation on this...
[^2]: I'm pretty sure I do need a citation on this

> With the recent case of [AWS](https://www.bloomberg.com/news/articles/2021-01-11/amazon-s-removal-of-parler-shows-cloud-unit-s-rarely-used-power) and [other cloud providers](https://www.businessinsider.com/parler-offline-hosting-ceo-john-matze-amazon-large-website-providers-2021-1) rejecting social media app Parler , there is an interesting precedent for companies boycotting certain content on the mainstream web. 

## Search Engines Are Trusted, but Imperfect
For context, I grew up viewing "the internet" as an auxiliary parent. I could ask Google for help on my homework, and find the answers to questions like "how to convert Celsius to Fahrenheit", "area of a triangle", or *super embarrassing questions* I was too uncomfortable asking other people. 

I don't think this is a crazy feeling either, as this [survey on statista (I don't have premium, so I can't see source information)](https://www.statista.com/statistics/381455/most-trusted-sources-of-news-and-info-worldwide/) show that search engines are the more trusted source.  

The reason why I'm emphasizing this is because google can show questionable results especially when it [comes to opinions](https://www.wsj.com/articles/googles-featured-answers-aim-to-distill-truthbut-often-get-it-wrong-1510847867) or [advertisements](https://www.nytimes.com/2020/01/31/technology/google-search-results.html).

## *Anecdotally* Search Engines Are Filled With Junk
From my personal experiences, it seems that google search results are becoming increasingly polluted with garbage. Medium articles and Youtube videos have become incredibly more common as "top results" for technical searches. 

It might be because my cookies indicate that I click these links, so I get them recommended more, or because other people click these links and use them as helpful resources. In the end, most of these resources are poor alternatives to actual documentation, papers, wikipedia, etc. 

Search engine filters can fix this problem (which I eventually had to do), but the most vulnerable group (inexperience users) will have to sort through the junk. 

## Everything on the Internet points to each other (but with almost no citations)

Internet content ends up copying content from textbooks, books, or other internet content. 

An infamous example is Siraj Ravel who's earliest (and current) "tutorials" plagiarized of other people's repositories, official documentation, and academic papers[^3]. An example I found was his ["CUDA Tutorial"](https://www.youtube.com/watch?v=1cHx1baKqq0) copying the contents of [Nvidia's own "An Even Easier Introduction To CUDA"](https://developer.nvidia.com/blog/even-easier-introduction-cuda/)

[^3]: There is a ton of videos and articles covering this drama - too much to link

A worse version of this is when content points to each other with poor citations - leading to propagation of unverifiable information. For instance, the commonly cited fact "bugs found in requirements are 100x cheaper than bugs found in implementations" is unverified and probably made up. [This blogpost describes the wayne's adventure discovering this information and the struggles of academia in empirical software research - I recommend it](https://buttondown.email/hillelwayne/archive/i-ing-hate-science/).

A non programming example that I was personally upset about is when [I made custom chocolate bars]({% link _posts/2019-8-29-custom_chocolate_mold.md %}). I received the inspiration from a series on making chocolate from (and a lot of the instructions from) the [youtuber "Alex"](https://www.youtube.com/user/FrenchGuyCooking). However, after searching a bit more on tempering chocolate (a process of heating and cooling chocolate to reach an ideal texture), I've discovered that the youtuber's methods (and information presented) are very similar to [information found on J. Kenji López-Alt's guide on chocolate](https://www.seriouseats.com/2014/12/the-food-lab-best-way-to-temper-chocolate.html). It doesn't discredit or remove any of the creativity or story-telling of the video, but it's slightly disingenious to present these innovative discoveries without proper credit. 

## Linking = Easier Verification

It's possible for conventional wisdom to be wrong about a subject, and for the general public to take a while to catch up with the more "nuanced" wisdom[^4]. What's wonderful is that normal people can verify (or at least be convinced) that conventional wisdom is wrong with evidence (Science!). Without evidence, I just feel confused. 

[^4]: I can imagine anecdotal evidences, but no citations with systematic studies of "conventional knowledge vs reality"

In this ["best practices" post](https://newrelic.com/blog/best-practices/expected-distributions-website-response-times) by NewRelic, a company that provides monitoring services and apis, the article states that one of their engineers knows that latency distributions more follow an erlang distribution rather than a log-normal distribution. Then there is absolutely no citation, evidence, or reasoning, so I'm left furiously researching (and trying to find results)[^5]. On the other hand, the same post links research detailing reasoning and methodology for why log normal distributions generally fit latency measurements (Ciemiewicz, David M. "What Do You mean?-Revisiting Statistics for Web Response Time Measurements." In Int. CMG Conference, pp. 385-396. 2001) - where I can *pretend* to understand the research's and share it. 

[^5]: To be fair, the individual probably was pestered into contributing to this and had other priorities

The engineer in question could be right - but without a link it's incredibly difficult to verify these claims. As a result, the a skeptical audience is left confused and unconvinced. 

## "Common Knowledge" is Rarely Common

Linking is an informal opportunity to curate learning resources and increase accessibility to the audience.

Unlike a journal, where the intended audience is experts, publishing to the internet tends to have broader audiences. A beginner/intermediate programmer can be interested in what you do. While this individual might know about classes, encryption, language paradigms they might not be familiar with other fundamentals topics like compilers, DNS, architecture, etc. While you could explain the foundational topics relevant to your post, that can be a lot of effort for a relatively little payoff. On the otherhand, linking provides the opportunity to provide quality resources for beginners to accelerate their learning rather than just having to awkwardly accrue knowledge. 

## People Believe and Repeat Incorrect Information 

I emphasize citations because I've had several "mhm" moments throughout my life. I would hear things that were incredibly dated, ignored context, or just incorrect.

These people (who I respect) might cite flawed statistics, faulty science, or lies. When I asked the individual for the source of that information (or quick google results), they would have a difficult time actually finding this information.

> I don't expect people to carry a bibliography for every bit of knowledge they have, but if one uses "science" to back a larger argument the least you can do is show *some* source.  

> Some examples I've heard with some evidence demonstrating otherwise: ["undocumented immigrants take more from government resources than they contribute"](https://www.pbs.org/newshour/economy/making-sense/4-myths-about-how-immigrants-affect-the-u-s-economy), ["According to the Laffer Curve, lowering taxes will actually increase tax revenue"](https://www.washingtonpost.com/politics/2019/06/01/trump-is-giving-arthur-laffer-presidential-medal-freedom-economists-arent-laughing/)

My experiences and the attention on fake news and disinformation in the 21st century is just a reminder how bad this problem has become and demonstrates the need to verify "facts" we here. 


Linking provides accountability to help deal with this. That way when a tweet or blog links "scientific study: `some_antivax_blog.net` (or fox news)", my peers and I can judge the reliability of the info[^7]. 

[^6]: But to be honest of my own biases, I'm a much more lax about verifying this information if I believe it

[^7]: Of course, I'm guilty of not verifying information that aligns with my beliefs.

# Weaknesses

I've spent most of this post arguing for more aggressive linking and citation patterns on the internet, but I know that's not always viable (or desired). A lot of details are not cited in this post because I can't spend more months researching and drafting a 10 minute read. If everyone followed a verified citation pattern, everything would just become Wikipedia (oh boy I wish) - but more importantly the work needed to maintain that isn't realistic for most people. 

Furthermore, you can't conflate the number of citations with the quality of the content. Plenty of peer-reviewed, published papers that end up being critiqued or rejected in the future, but that's the beauty of the scientific process!

Another flaw is that links disappear - a well crafted, tested website you used might exist for the next 3-4 years, but might noe exist for the next 10 or 20. 

# Ending Note

This was my reasoning (and opinion) for why we should pursue more rigorous citation styles with digital content. I always welcome discourse so reach out to me through email: `nablags97 [at] gmail [dot] com`.

