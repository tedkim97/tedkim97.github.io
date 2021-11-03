---
layout: post
title: Purchasing USB-C Pro Micros
subtitle: (and other life updates)
published: true
enable_latex: false
enable_d3: false
permalink: promicroc
frontpage: false
technical: false
funstuff: true
tags:
  - keyboards
  - typing
  - arduino
  - custom
  - design
concepts:
  - arduino
  - keyboards
---

I haven't been posting much recently because I'm taking cs229: machine learning at Stanford as a non-degree option. The class is a **lot** of work (but I would recommend, especially the lecture notes), so I had to (temporarily) cut time from my other endeavors such as (1) Work (2) CS229 (3) Social Activities/Hobbies (4) Blogging about Hobbies. Since work and the class are non-negotiable and maintaining a "healthy" mental state involves (3), I decided to cut down on high-effort blogging temporarily[^1]. 

[^1]: This break has given me more time to jot down topics/ideas for other posts down the line. 

I've purchased a few pro-micro USB-C devices from Aliexpress - essentially these are just a slightly modified version of the traditional Pro Micro w/ a micro-b connector. This is exciting for a couple of reason:
- The Pro Micro did not really have a cheap clone with USB-C until recently (August/October 2021? - I will track down a more precise date at a later time)
- I'm happier with my prototypes 

For some context, an old passion of mine[^2] is mechanical keyboards - with my intensity from just buying mass-manufactured keyboards to "DIYing" with different form factors, switches, keycaps, flashing with custom QMK configurations, group-buys, etc[^3]. While I don't have an excess of keyboards some people show on geekhack and /r/mechanicalkeyboards, I am embarrassed by the fact that I have more keyboards than I would realistically use. As a result, I've limited myself to building/buying components for a keyboard that was designed & built by me. 

[^2]: Since playing Starcraft II: Wings of Liberty (2010)
[^3]: but I would **never** buy an artisan keycaps - they can be cute or cool, but I think they're incredibly dumb functionally and aesthetically. 

Part of this DIY experience is having a convenient abstraction for the keyboard's microcontroller unit (the ATMEGA32U4). The Pro micro provides all the important components (that I don't entirely understand) that I would need to add to a barebones circuitboard (such as a timing crystal, pulldown resistors, etc).

I was unhappy with how the Pro Micro (a popular DIY part of custom boards) had a micro-B connector (just because I didn't like it aesthetically & because I preferred USB-C). Other enthusiasts seemed to agree because there are plenty of Pro-Micro variants called the ["Elite-C"](https://deskthority.net/wiki/Elite-C) specifically designed for keyboards (more exposed IO pins, better connector mounting to PCB, etc). While I wanted one it was incredibly expensive [(\~$18)](https://keeb.io/products/elite-c-low-profile-version-usb-c-pro-micro-replacement-atmega32u4) compared to a regular pro micro (\~$5-6). The higher price is because of more complicated designs and the USB-C connector, but in my mind that didn't justify a whole $10 bucks per MC (especially when I didn't need most of these pins). In my mind - I just accepted that I had to live with this tradeoff. 
  
[Beware - it seems that a batch of these promicros have some pin-misallignment issues described here in this reddit post](https://www.reddit.com/r/MechanicalKeyboards/comments/qi1k2f/beware_of_the_usbc_pro_micro_clones/?ref=share&ref_source=link).


Here are some pictures for reference:

{% include image.html imgpath="figures/promicro_c_side_by_side.jpg" alt="top-down view"%}

{% include image.html imgpath="figures/promicro_c_side_by_side_2.jpg" alt="horizontal view"%}