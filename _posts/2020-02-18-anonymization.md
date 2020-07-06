---
layout: post
title: Anonymization Schemes
published: true
permalink: data_anonymization
frontpage: false
technical: true
funstuff: false
---

* TOC
{:toc}

# Introduction
Ignoring the question whether people want "big data" in their lives or not, "big data" is going to be an increasingly important part of society. [As of now, facial recognition technology has been reeled back by big tech companies](https://www.vox.com/recode/2020/6/10/21287194/amazon-microsoft-ibm-facial-recognition-moratorium-police), but the reality is that people [don't need faces to identify you](https://www.scmp.com/tech/start-ups/article/2187600/chinese-police-surveillance-gets-boost-ai-start-watrix-technology-can). Even if legislators were to outlaw all tracking technologies, [tracking individuals is very easy with smartphone app location services](https://www.nytimes.com/interactive/2018/12/10/business/location-data-privacy-apps.html). Moreover, the (incredible) book "Weapons of Math Destruction"[^1], the author describes **even more** alarming practices in the big data industry and calls for a more critical examination of "black box" models that dominate our lives. 

[^1]: I also recommend taking a look at Cathy O'Neil's [blog](https://mathbabe.org/) and articles in Bloomberg.

I agree with her stance, and I would go as far as to say that we should add stricter regulations in the data world. This post isn't explicitly about "black box" models, but about how we protect data and privacy - mainly implementation of data anonymization policies. 

**NOTE:** This is post is a slightly modified excerpt from a bigger blog post I wrote on web scraping. I cut this segment out because a shorter post would be much more digestible for someone just looking for some ideas/explanations. 

# Anonymize/Pseudonymize Data!
A couple of big security leaks has shown us that we need much better practices in the data industry. I have a *personal* theory[^2] that security vulnerabilities and breaches are made not because an engineer did their job out of laziness, but rather because good security interferes with usability and is cumbersome to use. 

[^2]: Only a theory!

Plenty of people re-use passwords because remembering a unique password for each service you use is incredibly hard! If an individual tried, they would probably write down their passwords online or on a piece of paper - adding a different kind of vulnerability. Even though the security of the blockchain and cryptocurrencies has been proven uncrackable - [people can steal crypto](https://selfkey.org/list-of-cryptocurrency-exchange-hacks/) because of shortcuts users and exchanges make to make security "easy". It's easier to create one admin account to a database, rather than hassle with multiple user accounts with varying levels of access.

In a similar vein, I think people don't anonymize data out of malice or greed, but rather  because it adds seemingly "unnecessary" complexity to working with data. However, if we want a better regulated data economy, we need to start from the basics and follow better practices when sanitizing data. 

An example of a bad anonymization scheme is a lookup table (ex: john doe => 1, jane doe => 2). If you Anonymize data with this lookup table, you can recover the person's ID by reversing the lookup - protecting no one.

# Absolute Anonymization: Deleting the ID:
Just deleting specific identifiers of data point is the best way of maintaining anonymity (i.e phone numbers, SSN, names, usernames, etc). 

# But what if we need to perform joins, or need to attribute activity to a user?
Anonymization can interfere with common "big data" processing techniques. For example, we aren't able to easily match up user behavior across sources (ex: performing SQL joins across different datasets). Furthermore, deleting IDs hinders analyzing user behavior over time, user submissions, or examining group of individuals. 

There is another approach to sanitizing our data: pseudonymization. **Anonymization** prevents re-identification of a data point that can't be used to identify anyone while **Pseudonymization** allows information to be recovered **given additional information**. In that vein, let's explore Cryptographic Hashes and Salting as a way of sanitizing data.

# Anonymization/Pseudonymization: Salted Cryptographic Hashes:
Cryptographic hashing is a method of taking an input of any length and outputting a (seemingly)random fixed-size hash. The reason why they're important is that cryptographic hash functions have a set of properties that make it ideal for certain security applications. Fun fact, cryptographic hashing and salting are common practice in storing passwords. This is why a company can't tell you what your password is when you forget - the company doesn't know it themselves, just the salted hash of your password.

We will ignore the engineering of cryptographic hash functions and look at their properties, and why they might be helpful for our use case. 

A good cryptographic hash function should be: 
- **Deterministic** : Hashing some value with the hash function will always be the same w.r.t the value. (`SHA256('abc') == SHA256('abc')`)
- **(Practically) Collision Free/High Collision Resistant** : (Good) cryptographic hash functions are collision resistant, meaning you won't find two inputs that result in the same outputted hash (or a "collision"). In reality, collisions are *possible* (making them not *truly* injective), but very unlikely. Refer to [this stack exchange post](https://stackoverflow.com/questions/4014090/is-it-safe-to-ignore-the-possibility-of-sha-collisions-in-practice#:~:text=For%20instance%2C%20with%20SHA%2D256,second%20to%20about%2010%2D15.) if you want an idea of how unlikely.
- **One-way (No inverse)** : This means that it should be difficult to find the inverse of `hash(x)` besides brute-forcing all X's. Assuming you're using a popular hashing method, this should be given. If you could quickly invert SHA-256 you would be the most powerful person alive. This property is also called Pre-image resistance.
- **Avalanche Effect** : A change in a bit of the original input should drastically change the resulting hash - preventing any correlation between hashes of similar inputs.
- **Easy to compute** : this system wouldn't be very usable if it were slow to compute

With these properties, we have a potential anonymizing scheme that allows for consistent identification when hashing an ID (determinism), you cannot have faulty duplicate ID's (collision resistance), you cannot find the ID given a hash (one-way), you cannot try to solve for patterns through hashes (Avalanche effect), and it's fast (easy to compute). However, naked hash functions alone are not good enough.

In fact, there exists an easy attack to naked hashes. Since usernames are *meant* to be public - an attacker could run a couple of hashing algorithms on those publicly available usernames, and sees if any of the generated hashes match your dataset. This is basically an easier version of a popular encrypted password attack (the rainbow table attack)! 

As a result, a popular technique to circumvent this is to "salt" the input - which is the processing of appending information to the input, to completely change the output. An example would be `hash(x||blargh) != hash(x)`. Now as long the attacker doesn't know your salting patterns, they won't be able to find IDs as easily. 

Despite all this effort, you can't truly anonymize data as long as the salting technique and CHF are known (we can recover user IDs if we have a list of usernames and the salt+hash scheme). This paper "Introduction To The Hash Function as a Personal Data Pseudonymisation Technique" lists some strengths and weaknesses of using hashing as a pseudonymization scheme[^3]. 

[^3]: https://edps.europa.eu/sites/edp/files/publication/19-10-30_aepd-edps_paper_hash_final_en.pdf

Implementing cryptographic hash functions is complicated, so I recommend using a library (like `pycryptodome`). 

***Disclaimer***: this isn't a perfect solution! There are plenty of articles that point out the problems with this approach - mainly that it can't *truly* anonymize data. However, this solution works pretty well for a pseudonymization scheme.


# Conclusion
Overall, we've gone over the engineering and logic behind a more sophisticated pseudonymization scheme to protect user privacy (aside from just deleting IDs). This post is not necessarily a recommendation for this technique, but an exploration of its strengths and weaknesses. 

I want to emphasize that this pseudonymization scheme is definitely not perfect - nothing is perfect! It's important to analyze your approach to sanitation and realize that the devil is in the details (especially with cryptography). An example is that the ["One-time pad"](https://en.wikipedia.org/wiki/One-time_pad) **encryption** is provably, perfectly secure (uncrackable)! However, if you have multiple messages encrypted with the same pad, you can recover the pad, and then recover all the encrypted messages (from uncrackable to vulnerable through a slight misuse of the encryption scheme). 

Whatever approach you take, it's important to analyze the weaknesses your solution has. It's up to you to determine if the weaknesses are **acceptable** and prevent your schema from being misused.

# Footnotes