---
layout: post
title: Personal Dread&#58; Migrating Away From Jekyll 
published: true 
enable_latex: false
enable_d3: false
permalink: migrate_blog
frontpage: false
technical: false
funstuff: true
tags:
  - website
  - programming
  - ruby
  - software
  - complaining
  - headache
concepts:
  - SSG
  - migrations headache
  - complaining
---

I just wanted to talk about one of my concerns from working with Jekyll - **migrating to a different static site generator (SSG)**. 

I have this migration concern because I have a few personal quirks about my tools for personal projects that make me unhappy with Jekyll (language environment, maintenance requirements, etc). 


# Why Migrate? (Software Rots)

I mention migrations, because I recently read [this blog post detailing Jekyll's development freeze (since 2018)](https://www.bridgetownrb.com/future/rip-jekyll/). I'm not alarmed by the contents of the blog (although I do feel sad about Frank's passing), but the freeze has made re-evaluate my reliance on jekyll. 

To be fair, I don't **need** to migrate, but I'm not entirely satisfied with the "No Migration" option because
1. Software rots/decays
2. I have absolutely no interest in Ruby

What do I mean by software rotting? Well software can rot in a lot of ways[^1], but what I'm concerned about is the lack of updates for Jekyll. 

[^1]: I'm not the expert, so no linking from me

I don't want or need updates for new features. However, tools need to updated and validated for newer version of the programming language, operating systems, etc. Ruby is (probably) not going away, so compatibility with future operating systems should be guaranteed, but there is no guarantee that the same community will maintain Jekyll to the same standard. In theory as long as the language remains the same, or doesn't break backwards compatibility (like the conversion from Python 2 to Python 3) the functionality should be the same, but niche things break all the time so you never know. 

This isn't a deal breaker because as long as I host the build process in the cloud, pick a matching OS, older architecture, and older version of Ruby I should be able to use Jekyll just fine (at the expense of local development).

Another reason why I'm uncomfortable staying is because I have absolutely no desire to learn about Ruby. There are a lot of gross bodges on my blog to have small features, but I wouldn't need those if I just modified jekyll. While I don't need to understand Ruby to use the tool, if I want to change something or add new features, I would need to learn Ruby.

## Just Migrate to a Jekyll Fork

This is probably one of the easier solution - but my concerns with learning Ruby are still there.

## Making my own SSG

To solve the programming environment complaint, I've thought about programming my own generator. I know *enough* about HTML, CSS, JS, parsing, threading, substitutions to attempt to recreate what Jekyll in a language I like. Since I know my needs (at least I think I do) the tool doesn't need to cater to every possible need someone might have. If I'm lucky maybe other people would like the tool and contribute to the project.

These delusions clear up after 10 seconds because the reality is that I would be making less powerful, less efficient, less tested, less usable version of a lot of SSGs out there. 

## Migrating To Hugo

In the end I *might* migrate to Hugo (because I like golang and plan to use it more). There seems to be plenty of automatic migration support from alternative[^2] and people detailing their experiences[^3]. 

[^2]: https://gohugo.io/tools/migrations/
[^3]: https://chenhuijing.com/blog/migrating-from-jekyll-to-hugo/ and https://www.dannyguo.com/blog/migrating-from-jekyll-to-hugo/ 

The reason why I haven't pulled the trigger is because one pain is the conversion of my jekyll pure solutions (generating related pages, including d3 & latex sources, placing pages in the right environment based on front-matter properties, etc). It's not exactly clear how simple/complicated the substitutions would be, but there might be a lot of them. 

The bigger pain is validating that everything is generated correctly. Save every page with both versions of the generator, and comparing the HTML isn't a perfect solution because the HTML might be syntactically different but semantically the same, the CSS could be broken, etc. The ultimate validation is going through every page manually and making sure everything works correctly.  

The sad part is that the more I blog, the more work I'm creating for myself down the road. 

## (Nothing to do with Jekyll) Migrating away from Github Pages

Free hosting from github is awesome, but something I plan is to move to a different hosting provider. There is no real rationale behind this desire except to feel like a big boy developer.

# Conclusion 

That's it. 

I want to migrate away from Jekyll into an environment I find more comfortable - like Hugo. However, migrations are a lot of work so I'm delaying on pulling the trigger. 

# Footnotes