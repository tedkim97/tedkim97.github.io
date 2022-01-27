---
layout: post
title: Mobile Medicine Retrospective
published: false
enable_latex: false
enable_d3: false
permalink: mobilemedicine
frontpage: true
technical: true
funstuff: true
tags:
  - website
  - programming
  - software
  - java
  - backend
  - frontend
  - architecture
  - design
  - complaining
  - headache
  - research
  - medicine
  - healthcare
concepts:
  - public-health
  - systems
  - research
  - reflection
---

This is more of a brief retrospective on a project I worked with for [Professor Pedro Lopes](http://plopes.org/) and [Professor Andrey Rzhetsky](http://med-faculty.bsd.uchicago.edu/Default/Details/10517) - rather than a technical breakdown. The repository where the code lives is [here](https://github.com/humancomputerintegration/realtime-diagnostics-app) and the Arxiv is [here](https://arxiv.org/abs/2110.15127). From my understanding there might be a publication in PLOS down the line. 

# Background

This was the summer after working in Finance internships and discovering how much I did not enjoy that career track - 

and my skills were woefully inadequate for a software engineering intenrship.


# Designing Architecture From Weird Components

I think one ofthe best parts of computer science is "abstraction"

To be honest, I was a little confused why my systems course used the word "abstraction" so much - but it works very well with computers.

In essence, designing an architecture 
 


# MISC

Current diagnosis systems are either not accurate enough or require too many resources,
rendering them unusable in these markets. 

We are collaborating with the African Institute of Mathematical Science and the University of Ibadan to design and deploy this electronic adaptive
diagnosis system in order to help healthcare providers in Rwanda and Nigeria, as well as other parts of the world, such as Pakistan. This system is a lightweight, adaptive diagnosis assistant system designed to help healthcare providers arrive at diagnoses faster and with less errors and to assist with efficient patient record keeping. The adaptive diagnosis assistant system is based on online learning algorithms and will improve by itself as more patient data accumulates.

We developed this Android application prototype for use in clinics, and beta- tested it with five doctors in Pakistan. The performance and feedback were mostly positive, with an accuracy of 68 percent, and the doctors reported that the application was very user-friendly, and they liked the idea that it mimics a
physicians’ decision-making process. As to be expected, the doctor-to-patient ratio is extremely small in
developing countries – doctors in rural or urban areas of Pakistan have huge patient loads with which
they struggle. 

It is therefore important to provide a tool that can help improve their diagnostic efficiency
and hasten their workflow, enabling them to potentially attend to more patients per unit time.

Our diagnostic application is specifically designed for lower-resource settings. While the application
targets lower-resource settings, such as rural Nigeria, the US is not devoid of lower-resource areas. In
the US, annually, over 12 million adults who seek outpatient medical care receive a misdiagnosis [22],
and there nearly 30 million uninsured individuals (ten percent of the population) in the US. Our
application may be able to help these people as well.

In lower-resource settings, particularly in countries ranking low on the Human Development Index, obtaining accurate and timely medical data is extraordinarily difficult. Without robust local datasets, the function and evaluation of our diagnosis system is limited in its present form. Although we have utilized
large data sets from the US and experimented in Pakistan, it is unlikely that these data can properly represent rural African areas. We are in the process of deploying the application to countries like Nigeria to conduct a first trial experiment to collect more localized data, recognizing that there is vast
heterogeneity across the African continent and also within countries -- especially between urban and rural contexts.

In the meantime, based on the feedback of doctors who tested the assistant system, it would be very useful if medical data could include more fields such as the duration and evolution of symptoms or patient medical histories. These are important factors that would help doctors arrive at better diagnoses
and treatment plans. However, in developing this ideal system, we are restricted by both insufficient data sources and technology. The application requires a large amount of related data to add the requested features or medical fields to the system, and different algorithms are needed to handle the
time-series information. We are planning to upgrade the system in this direction after the next trial in Nigeria.


Network accessibility may also be a problem when deploying the diagnosis system. To the best of our
knowledge, cellular networks in many African rural areas are slow and brittle, and some rural areas have
no cellular coverage at all. However, we did our best to build the lightest and most reliable diagnostic
system possible. Consequently, the current version of the system only requires sending SMS messages
of dozens of characters for each visit. Theoretically, this matches what we have seen so far in the field,
but we have not tested the system while connected to an extremely unstable network; it is possible that
data synchronization may fail. As we continue developing our AI assistant system, adding additional
facets of patient health descriptors, the messages to be sent will grow larger, creating logistic difficulties 