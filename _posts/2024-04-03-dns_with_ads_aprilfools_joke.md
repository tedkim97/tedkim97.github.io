---
layout: post
title: My April Fool's Joke made it to the front page (of hackernews)
subtitle: A DNS resolver with advertisements
published: true
enable_latex: false
permalink: dns_with_ads
frontpage: true
technical: false
funstuff: true
tags:
  - april fool's
  - DNS
  - networking
concepts:
  - networking
  - rust
---

My april fools joke made it to the front page of hacker news: https://news.ycombinator.com/item?id=39895453!
- [Git Repo](https://github.com/tedkim97/adcache).

{% include image.html imgpath="figures/adcache/after_1_hour.png" alt="on the front page after one hour"%}

{% include image.html imgpath="figures/adcache/after_4_hours.png" alt="on the front page after four hour"%}

The april fool's joke was that I was a pretend-SAAS company that was "innovating" [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) by providing an advertisment-supported DNS resolver - where we would randomly inject ads for the users. I.E when a user made a DNS query, advertisments would pop up like so:
```
dig @35.223.197.204 hackernews.com

...
    # Actual DNS record
    hackernews.com.  46 IN A 13.249.141.39    
...
    # Advertisment
    hackernews.com.  7200 IN TXT "Meet hot, lonely DNS records in your area tonight"

```

For the unfamiliar, DNS is very technical, foundational part of public internet and networking - essentially, something that no one would ever ask for ads in. If you can't tell... this is a VERY niche joke.

I initially had an idea for this joke back in early 2023 - I had a vague idea that "making DNS with advertisements would be so bad it's hilarious". I ended up spending a decent chunk of time on the project, and I was a bit nervous that it would fade into obscurity, but I'm glad it got some attention and people found it funny.


## The time investment 

It was surprisingly stressful to work on this project... because trying to be funny can be stressful. When it comes to jokes I constantly go through this seesaw of "it's funny" to "it's cringe" - moreover, I was also worried about making jokes that wouldn't offend anyone (not that this is a particularly touchy topic). 

## Explaining the joke/satire

I'm going to do the thing you shouldn't do... explaining the joke. The audience for this is incredibly niche, so I think it's fair to make the joke more accessible for bigger audiences. 

I sorta wanted to make a bunch of tiny jokes about trends you see in the software industry, something that might not be picked up to all audiences.

On one level, the jokes in the DNS records are obvious:
- "Meet hot, lonely DNS records in your area tonight"
- "I make $100,000 USD every month! Buy my course to find out how!"
- "This response is sponsored by Raid Shadow Legends"

Other jokes are more "programmer" niche:
- Devs constantly migrating to Rust or bragging about their "blazingly" fast software was written in rust
- The [enshittification](https://en.wikipedia.org/wiki/Enshittification) of products over time. 
- Providing a free service, but also providing a terrible, paid subscription on top of it (think Youtube Premium that still has ads...)
- Collecting & selling user data 
- The stochastic entrepreneurial process of inserting advertsiements into *anything* to try to make a big business out of it

Some of the jokes are just me complaining about boring ads are
- "This response is sponsored by $MEAL_DELIVERY_KIT! Use this coupon to get $2 off your next order!"
- "Watch the action-packed, romantic, comedy of the century $MOVIE in theaters near this summer"
- "Welcome to the metaverse! Come here to buy real-estate in the metaverse"

The final funny bit I like is:
- Why would anyone waste their time putting so much effort into an april fool's joke? 


### Delivery

The delivery of the joke (besides setting up an actual DNS resolver that anyone can use), is intentionally overt.

One comment pointed out "Little over the top. Sometimes subtle is better/more entertaining" - which I completely agree with (I didn't address it because I was "playing the joke" in the comments). I think I would have liked a more subtle tone to the overall joke - to make the satire more subtle.  

However, I consciously made the delivery very blatant becasue I was nervous that people wouldn't get the joke... When I shoed initial versions of the marketing material to friends, some of them didn't get it was a joke. They certainly weren't stupid and they were engineers, but the the specificity can make it difficult to see the humor. Sarcasm isn't clear when you don't have tone to help emphasize the point. 

So on the last day I decided to add emojis, more obvious jokes that i's a cash grab, etc to prevent ANYONE from POSSIBLY misconstruing this has a serious product. 

Technical breakdown of how I programmed/deployed/architected this joke coming soon. 