---
layout: post
title: Testing&#58; A Tragedy 
subtitle: A mediocre short story
published: true
enable_latex: false
permalink: testing_a_tragedy
frontpage: false
technical: false
funstuff: true
tags:
  - programming
  - opinion
  - microservices
  - software
  - architecture
  - testing
  - deployments
  - environments
concepts:
  - short story
  - mediocre
---

# Context
This was a "story" I wrote to illustrate a point about testing within complex software architectures in a different blog post. The story got too long, too cringe-worthy, too difficult to make "good" - and was distracting from the actual purpose of my original post. As a result, I decided to cut my losses and quarantine it to a different post. 

The gist of the story is that testing comes with a lot of little & large frustrations that are too difficult and erratic to describe, you just need to "feel" them. Hopefully, the intended audience will know the pain that I'm talking about.

# "Testing: A Tragedy" 

As a story telling exercise, let's pretend we're a developer working in a trendy "service-oriented" architecture. 

We're jazzed because we just wrote a clean implementation for $FEATURE. Wanting to follow good SWE practices, we plan to test our changes to check the correctness of our implementation. We think, "this should be easy", as all we need to do is pull the latest images for each service and make sure their configurations are correct. 

We spend \~15-20 minutes parsing through configurations, dockerfiles, scripts and double checking to make sure everything is correct. Some of these services have a "dependency" order - meaning we need to make sure Redis boots before `Service1` and Kafka boots before `Service2` and `Service3`. We diligently consult our notes and start everything in the right order. Our feature may involve a different team's service, so we spend 30 minutes downloading their repos, following their docs[^1], and praying that they didn't skip a tiny but critical detail in their documentation. 

[^1]: docs is shorthand for "documentation" - a magical scroll that only exists in myths    

While we were focused on setting up the new repo, our Kafka container failed to build and now something is error-ing about schemas. In the back of our minds we have to worry about all of the possible reasons why this random failure occurred. Regardless, we're annoyed as we have to manually kill containers, and hope a reboot will fix the error.

With a nag in the back of our mind that something is misconfigured in our local setup, we start testing. After spending another hour diligently testing and documenting our tests for reproducibility, we are confident enough to make a pull request. After 1-2 days asking for reviews & approvals we merge our changes and push to DEV - reproducing our tests from earlier. Testing in DEV goes great and then we push to INT!

Tragically, we discover an error that occurs 25% of the time in INT. The issue involved is because our changes didn't account for all of the authorization flows, and failed to properly handle the resulting errors. We lament our foolishness for missing something so obvious! "$SENIOR_DEV" wouldn't have made that mistake.

With this new bug in mind, we write some more code to fix our issue. Instead of trying to reproduce the error in our local environment, we end up having to modify our local versions or directly injecting the behavior in our code. Of course that's not the "correct" way of testing out fixes, but we're already 1 day over our sprint value. We restart the 45 minute process of setting up the local environment. We spend another hour diligently testing and documenting our tests for reproducibility, and are ready to make another BUGFIX pull request. After an urgent day asking for reviews & approvals, we merge our changes and push to DEV and INT.

After pushing to STG, we discover an error in STG that occurs 5% of the time. Processes are mysteriously failing and hanging for cryptic reasons... Even $SENIOR_DEV is confused. We spend 2-4 hours investigating and eventually discover that the Payments service sends requests to cancel, causing $EXTERNAL_SERVICE to delete database entries before we've read from them. 

We write more code to fix our issue. We're clever, and we reproduce the error by setting breakpoints/timers in our local service, and make queries to delete entries from our database. The process is a little annoying because of the processing of having to constantly juggle different terminals and IDEs. However, our local testing goes great! We create a bugfix PR, ask for reviews, wait 1-2 days, and merge the branch and push to DEV, INT, STG Again.

Everything in DEV, INT, STG goes great, so the team decides to push to PROD. And then something goes wrong...

Burned by our experiences of implementing and deploying our $FEATURE, we start getting lazier and lazier with our testing. We begin to haphazardly test and merge our changes in - relying on the integration and end-to-end tests to catch any bugs (but we know they won't catch all of them). Our ability to develop tickets starts slowing down as we starting saying things like 
- "I'm waiting for a meeting with $SENIOR_DEV to discuss any cases I missed in my pull request"
- "I've tested my branch with mocks, so I'll do more testing once I push my merged on DEV & INT
- "oh I can't test XYZ because it's really sensitive to environments, so I need to deploy my changes to INT for more reproducible/accurate conditions testing".

As developers we've accepted the fallout of software complexity, completing tickets, making bugs, and fixing bugs.

The end
