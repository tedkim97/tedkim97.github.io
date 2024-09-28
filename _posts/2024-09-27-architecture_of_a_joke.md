---
layout: post
title: Architecture of a Joke
subtitle: (operations and other stuff)
published: true
enable_latex: false
permalink: architeture_of_a_joke
frontpage: true
technical: true
funstuff: true
tags:
  - programming
  - software
  - rust
  - architecture
  - cloud
concepts:
  - programming
  - cloud
  - architecture
  - rust
---

# My April Fool's Joke: Ad Supported, Subscription DNS

[My april fools joke (briefly) made it to the front page of hackernews]({% link _posts/2024-04-03-dns_with_ads_aprilfools_joke.md %}). 

{% include image.html imgpath="figures/adcache/after_1_hour.png" alt="on the front page after one hour"%}

The joke itself is small, but what I think is more fun and interesting are the constraints I had for this project, and how there are a couple of clever decisions that affect the architectural, implementation, and operational choices.

I'm going to write down as many details as I remember + the details I wrote down after the event had concluded. 

# Spoiling (explaining) the Joke

The core of the joke was that this was another tech product, while technically impressive, is for a business need that people don't really want or need. I.E it's a "bad" product.

 There's also small jokes making fun of memes in the programming community like:
- Rust is "blazingly fast"
- Rewriting a product in a different language is a marketable benefit...?
- Companies constantly make product promises that they inevitably break
- Companies building products just to try to sell your data
- Companies breaking specs to make money

## The Inspiration

This idea was inspired because my workplace semi-regularly pushed "hackathons"/"call for product proposals" within my org. Being on a DNS team, there's not a lot of room for "out of the box" DNS products... I would regularly joke that we should do "LLM DNS", or "DNS with Advertisements".

## The History

As a fun little fact, [DNS hijacking for advertisements isn't even anything new](https://en.wikipedia.org/wiki/DNS_hijacking#cite_note-wired-isps-error-page-25). There was another blog link talking about this and I bookmarked it (but I somehow lost it). I'll update this page if I ever do re-find it. 

# The Requirements

If you want a quick summary of the requirements, they are:
1. Relatively small implementation time. I don't want to spend hours implementing a complete DNS resolver (this is where the clever part in the architecture comes in)
2. Performant. Ideally this service should be able to handle large loads and minimize latency to impress my fellow nerds (also in case someone decided to spam the resolver).
    - "Performant" is intentionally vague which I'll expand upon later.
3. Seemless operations. I don't want to carefully monitor + extinguish fires in case the resolver breaks for any reason. April fools was on a business day this year and I couldn't be distracted from work to find my personal computer/ssh/whatever ad fix it.
4. Reasonable spend on services. Excluding my time, I am not going to spend 100's of USD on a 1 day joke for clout among internet strangers.

# How The Architecture Saves Developer Time

The problem with making a joke like this is that making a DNS resolver from scratch is a lot of work. There's a billion reason for this, but mainly 
1. The DNS spec has a lot of nuance and detail
    - This is means there's a lot of testing that you need to do
2. There's a lot of caching you need to add in

While I appreciate RFCs, I don't think spending hours carefully reading, testing and following a spec is a fun april fools joke. 

While thinking about whether or not I wanted to go through with the idea of this joke, I realized there's a more devious way of getting a full DNS resolver functionality. Instead of programming a resolver from scratch, it's easier to develop a program that receives DNS queries & forwards them to an actual resolver (like BIND9), capture and edit the response, and return it to the user. 

This way, you can guarantee that the DNS resolution behavior is at least as correct to the spec as can be... before you ruin it and add a bunch of advertisments. Moreover, we can profit by reselling open source projects, just like the big companies :).

{% include caption_image.html imgpath= "/figures/adcache/adcache_architecture.png" alt="theoretical architecture of the joke" caption="Theoretical Architecture of the joke"%}

The actual execution of this was different from the end goal made from last minute testing (expanded upon later). 

# [Implementation Details](https://github.com/tedkim97/adcache)

In the end, what I need to program was a interceptor that just injected ads. To meet my performance goals, I didn't want the program to bottleneck the performance of the underlying DNS resolver. In essence, I wanted the throughput of the advertisement interceptor + resolver to match the throughput of the plain resolver.

I planned to do this by: 
1. Minimize copying of buffers
2. Reduce serializaition/deserialization between wire-encoded format & logical structs

To get as much compute throughput as possible!

The above was completely overkill and unnecessary and made testing + debugging a huge pain. This was unnecessary because the resolvers I was testing with ended up being more of a bottleneck (more on this later).

## What about TCP & DNSSEC?

I didn't feel like dealing with TCP forwarding, so I just made my resolver reject all TCP requests and return a response saying "TCP REQUIRES A SUBSCRIPTION". In retrospect it would've been funny if I added a rick-roll in the message.

I could've handled DNSSEC. Since the underlying mechanism for resolving queries is a real DNS product, I could just forward the details. However, I was worried about message sizes overflowing + didn't want to deal with the headache, so I rejected all DNSSEC queries and returned a joke.

# The Execution/Operations

Initially I was going to set up port forwarding on my internet router and forward traffic to my desktop. However, I wasn't sure if that was going to fit my goals of "seemless" operations. I was nervous of what my ISP would do when they notice a sudden influx of packets coming into my residental IP; moreover, I wasn't sure if I could even use port 53 of my router for DNS. At the time I had roommates, and they were definitely not going to be happy if our apartment's WIFI wasn't working because of some random shennanigans I decided to do.

As a result I decided to host this service in the *cloud*. I chose GCP because I was more familiar with that. Moreover, GCP VMs have a lot of monitoring out of the box, so I could track how many packets were coming in/out and use that as a proxy for how much traffic my joke was receiving. 

I deployed this one ONE region in the midwest. I made a guess that most of the hackernews audience was based in the US to have reasonable latency to the general US population. Depending on my audience, maybe picking a region in the west coast may have been a better idea.

## Which DNS resolver did I end up using?

Originally I planned to use Bind9 as the true DNS resolver. However, I was cheap and thought I could save money by using an external resolver that I didn't have to host. My goal was to forward queries to DNS resolvers on the public internet like Cloudflare's `1.1.1.1` or Google's `8.8.8.8`. 

While perf testing with `dnsperf`, I discovered that these resolvers rate limit you pretty quickly (duh). Then I had an idea to use my VM's DNS resolver (https://cloud.google.com/compute/docs/internal-dns) to resolve my queries, which ended up increasing my throughput ~5x.
- Of course, I would NOT recommend this solution for ANY production scenario  

# The Cost
For roughly 24 hours of operation, the total bill ended up being $8.67 which included the IP Address + the VM. I don't remember which model of the N2 I ended up choosing (`n2-standard-16`, `n2-standard-32`, `n2-standard-64`) but it ended up being completely overkill for the task. 

# How many people used the service?

I did get a decent amount of traffic - I even took screenshots and did some back of the napkin math. Also note that not all of the traffic I was receiving was from people resolving DNS traffic. Some of that traffic is just random crawlers/bots on the internet. 

As I mentioned earlier, the amount of compute I provisioned for this service was completely overkill.

# MISC Notes

## Anycast-ing
It would've been cute if I were able to have anycast support. Anycast would help meet the performance goals for international audiences. However, I wasn't able to find a way to support anycast with 1 IP address and multiple machines with GCP (without using an additional loadbalancer product)[^1]. Moreover, I wasn't going to fork over the cash to support such a niche functionality. [There was a comment saying that reddit.com took 11.4 seconds to resolve](https://news.ycombinator.com/item?id=39895453). I guess that this person was from a far away location from the server.

[^1]: Granted I put it only 30 minutes of research

## Monitoring
I had this idea of tracking all the unique IP addresses that used my DNS resolver; however as the deadline got closer, I decided to cut that part out because I didn't have enough time.

# The Joke Execution

Ironically one of the toughest decisions for this project was to decide on how subtle to make the humor.

I prefer subtle, dry humor, but I wasn't confident the audience (technology enthusiasts/software engineers) would understand the joke if they *weren't* over the top. One hackernews comment pointed out "Little over the top. Sometimes subtle is better/more entertaining" which I completely agree with.

# Conclusion
Overall, it was a fun project and I'm glad to see there were people who liked it and thought it was funny. See you next year!