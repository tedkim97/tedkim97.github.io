---
layout: post
title: Can there be too much OOP?
subtitle: Opinionated argument for chilling out with objects
published: false
enable_latex: false
enable_d3: false
permalink: too_much_oop
frontpage: true
technical: true
funstuff: true
tags:
  - programming
  - communication
  - software
  - case-study
concepts:
  - Language Paradigms
  - Object Oriented Programming
  - Functional Programming
  - Manageability
---

# Introduction

This is a very neutral opinion on language paradigms in the industry.

## Parts of functional programming that aren't necessarily core to programming, but I would really like in OOP

Unions
Match/Case statement
Statelessness

Expressiveness - anecodatlly, it feels very clunky to program even really simple things with .NET

## Parts of functional programming that I dislike 

Abstraction/distance from actual efficient computing
- The textbook Functional Data structures shows how you can manipulate/construct functional data types so that they are just as perfomrant as their procedural counterpoints
- Function purity: 

```
let a = 1; 
let b = (operation1 a 2);
let c = (operation2 b -1); 
```
the compiler might optimize as c = b 

Goddamn recursion 
- I hate how it's 

# Interesting Talks

# Awesome Environments

Elm is the worst - to be fair, I was learning elm when I was 
- I would have much prefered to learn an actual programming language for the course

.NET F#: 
- F# is a pretty wonderful environment to write functional code
- I feel the interopability between F# and other .NET environments is a bit exaggerated, it works but it is a bit clunky to use and it's not the ultimate thin 