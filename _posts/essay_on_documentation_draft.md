---
layout: post
title: Abstractions, Curb-Cutting, Literacy, And Documentation
subtitle: Documentation is complicated
published: false
enable_latex: false
enable_d3: false
permalink: ilovedocumentation
frontpage: true
technical: true
funstuff: false
tags:
  - programming
  - python
  - communication
  - documentation 
  - abstraction
  - software
  - metrics
concepts:
  - essay
  - documentation
  - leaky abstractions
  - accessibility
---

## https://javascript.plainenglish.io/the-ultimate-guide-to-writing-self-documenting-code-998ea9a38bd3

# WRITING NOTES: Thesis/Outline
Thesis: Documentation is important (duh) because it can greatly ease the usability of the code base, and is a way of greatly increasing the speed at which people get up to speed with it. 

I don't think documentation is going to greatly increase our ability to update old code, but it's **better than nothing**

- **Abstractions, Leaky Abstractions** - what they're for, why they don't work sometimes, and other stuff
--- The reason why I bring up abstractions is that "modern" programming relies on them (hopefully this isn't a controversial opinion). 

--- **Other stuff**
--- Programming Languages are an abstraction themselves
----- Programming Languages are an easier way of expressing our high-level instructions, into machine executable chunks.

- **Self Documenting Code is Sufficient** - self documenting code is a nice pattern, but I think isn't sufficient
--- The reason why I think it's insufficient is because
----- "Optimal" code can be incredibly complex
----- Programming Literacy can vary from contributor to contributor
-------- A new chunk of engineers come out of tech training without necessary the old understanding of coding. This is fine and they're able to significantly contribute over time
----- Programming Language and standards change *a lot* over time. Using C & C++ for example - these are programming languages that have over decades of provable history. Things that were de-factor and industry standards 20 years ago is iconsidered bad-practice, or verbose today.
-------- New Programmers on a newer standard of C++ for example, wouldn't understand why certain patterns existed.
- **Counterpoint: You never code in a vacuum: Why do you need thorough documentation when you have coworkers to answer the question?**
------ Coworkers can forget the complexity and nuances of the code they've written 
------ Coworkers can leave the company, or take bigger responsivilities such that they can't field your dumb silly questions
------ Even with open-source projects, you can ask questions too the experts on contributors 
- counterpoints and considerations: Economics of documentation

### What is the ROI on documentation?
Timelessness?
A fallback/backup?
Way to ease the pressure on developers/new people

I've been taking a break from typing a lot because of some wrist issues, and work getting busier. 


This blog post is a bit different because rather than being factual or a description of what I've done - I'm going to do a scary thing by making **claims** on the internet and use these claims to form reasons. 

**What i'm going to talk about**:

This dev talk is going to be a bit "wide" in terms of technicality. Rather than being a deep-dive into a specific topic or idea, I wanted to form more of a conversation around documentaiton practices.

# Abstractions, Curb Cutting, and Documentation



# Computers Are Complicated & Abstractions Are Necessary.
# OR: Modern Programming Relies on Abstractions

In order to be productive, programming relies on previously developed abstractions to express new ideas 


## Some Popular Abstractions


- TCP/IP
- Operating Systems: An Abstraction of hardware 
- Programming Languages: An Abstraction of instruction logic

## Leaky Abstractions
Leaky Abstractions are failure of abstractions to abstract X thing. 

This programmer defines a law that all significant abstractions are leaky abstractions. 

### examples of bugs coming from leaky abstractions
The numpy stack overflow thing I fixed


## A non Technical example
Me and trying to whip cream. I once stood in the kitchen swirling a wisk around really quickly - thinking that this was how you "whipped" cream. 

Nevermind the intense confusion I had about the semantics of cream/heavy cream/whipping cream. 


# Conclusion:
We've established that all significant abstractions are leaky.
Self-documenting code is not enough to fix these leaky abstractions.
Documentation is a way of making it easier for other people to get used to your code. 

# The "economics" of documentation: 
Documentation is a decent amount of work with maginal incentives for the programmer

Documentation ages very quickly - code can change/evolve so quickly that documentation a month ago can become quickly outdated. Furthermore, Heavy rewrites become heavier as the programmer has the burden of updating the documentation. 

Complete documentation and explanations only scales well with the more collaborators the program may have/will have (i.e open-source projects can have a couple of hundred contributors)  


Documentation is a nice benefit for onboarding.
Documentation is a nice benefit for time. 


### Auto-Docs Are Nice
Couple the code and documentation (at the cost of making the source file horrific imo) (python docstrings, Visual Studio XML documentation)

# "Self-Documenting Code"

The reason why I bring up these previous examples is because the philosophy "self-documenting code"
- Self documenting code relies on consistent naming, clear logic, etc 
- Following a set of McMaster conventions allows us to write documentation that communicates more without 

The problem:
- People need to follow these conventions (I often find myself making mistakes such like that)
- People need to understand these conventions/get used to them (Given our incoming cohorts and the context of tech training, maybe we should assume that everyone **doesn't** understand what's going on)


Example of self documenting code
```csharp
public int calculateSomething(List<int> someCollection) {
	// do complicated things
	return complicatedResult;
}
```

However, the reality of this complicated. There is almost some complication (edge case, or bug) that makes the code more complicated

Luckily, the code can still be self documenting. 

```csharp
public SomeResultClass calculateSomethingPrime(List<int> someCollection) {
	// do complicated things
	// edge case regard length of input
	if (someCollection.Length > UINTMAX) { throw new Exception(""); }
	// special values we want 
	if (someCollection.Any(val => val < 0 || val == 99)) { throw new Exception(""); }

	//  class that returns the result, with necessary metadata  
	return new SomeResultClass(complicatedResult, abc, d);
} 
```

# McMaster Culture
Everyone at McMaster is super firendly and more than willing to help. But sometimes it feels a bit awkward to burden other people with questions. 



# Looking at some good examples of documentation: NUMPY


# The reason Why I bring this up 

# Understanding that Documentation is not **the** solution to onboarding
Having clearer documentation won't solve onboarding/new people from asking questions about the architecture, code, niche knowledge, etc.

It just allows people to find these reresources faster

# Code & standards are not timeless
Standards, compilers, standard library functions change. 

As a result, the communication associated with them change as well. 

What was "semantic" in C 20 years ago, isn't "semantic" now. 

Something as trivial as curly bracket placing ({}) has changed over time/languages
```c
// I would be shamed for using K&R settings
int func(int i) {
	return i * i;
}
```


# Links
https://en.wikipedia.org/wiki/Leaky_abstraction

https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/

https://www.cs.swarthmore.edu/~kwebb/cs31/s15/bucs/chapter03.html
https://numpy.org/doc/stable/reference/index.html
https://numpy.org/doc/stable/reference/internals.html

https://github.com/numpy/numpy/blob/main/numpy/core/einsumfunc.py

https://karpathy.medium.com/yes-you-should-understand-backprop-e2f06eab496b

https://missing.csail.mit.edu/2020/version-control/