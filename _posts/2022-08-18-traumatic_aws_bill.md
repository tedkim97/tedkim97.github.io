---
layout: post
title: My Accidental AWS Bill
subtitle: And Other Warnings
published: true
enable_latex: false
permalink: big_fat_aws_billing
frontpage: true
technical: true
funstuff: false
tags:
  - programming
  - software
  - opinion
  - microservices
  - software
  - architecture
  - cloud
  - security 
concepts:
  - cloud
  - security
---


My AWS bill seems a bit larger than the usual (pictured below):


{% include caption_image.html imgpath="figures/large_aws_bill.jpg" alt="my anonymized, surprise AWS bill around 13000 dollars" caption="my anonymized, AWS bill" %}

My personal AWS usage is usually $0 per month, so a charge around \~$13,XXX was a bit surprising. Unlike other stories where surprise AWS charges come from configuration or architectural mistakes ([here is one I can remember off the top of my head](https://www.troyhunt.com/how-i-got-pwned-by-my-cloud-costs)), these charges were a result of a security mistake (I made). 

# Credential Stuffing

I seem to have been a victim of a ["credential stuffing" attack](https://en.wikipedia.org/wiki/Credential_stuffing). I made my personal account when I was in undergrad to get more experience with the "cloud technologies" trend. Unfortunately one of the password variations I picked was compromised in other password leaks. 

As a result, a malicious user hijacked my AWS account and ran heavy amounts of AWS sage maker (their machine learning platform) in all the default available regions[^1].

[^1]: What these trained models are used for - I'm not sure

{% include image.html imgpath="figures/breakdown_bill.jpg" alt="A breakdown of my AWS bill"%}

# Amazon Responses & Resolution 

Luckily for me, Amazon was incredibly helpful during the whole process and issued a one-time forgiveness for the bill.  

Amazon detected this malicious behavior fairly quickly and alerted me that their was suspicious activity on my AWS account. Unfortunately I was on vacation, so I was sloppy and inattentive about auditing my AWS resources - checking the services I used like EC2, only checking US-EAST-1. 

After responding to the first email and getting a 3rd reminder email I realized something was very wrong and checked the billing. 

It was a very uncomfortable 2 weeks knowing that I might be on the hook for a lot of money that I didn't use. However, AWS support helped me revoke all keys, permission groups, and resources in all of the major regions - as well as talking to the billing department to give me an exception for this mistake. 


# Takeaways

My initial reaction to this situation is that a beginner should avoid making cloud service accounts until they actually *need* it. However, I realized that this isn't the correct takeaway. Mucking around and experimenting with actual cloud resources (AWS, GCloud, Azure, etc) in an interactive approach is one of the most effective way to have ideas "stick". 

The actual lessons is that when one makes an account they should have these considerations in mind:
- Enabling 2FA
- Setting up cost and resource monitoring and alerts
- Setting up proper users and permissions 
- Disabling certain regions 
- Rotating passwords (although I find this personally annoying)

This lesson also serves as a reminder of how scarily effective cloud services can be - in a span of a few hours someone can easily spin up hundreds of VMs in all parts of the world and create (or destroy) a lot of value. 