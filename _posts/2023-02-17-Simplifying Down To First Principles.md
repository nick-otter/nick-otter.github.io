---
layout: post
title:  "First Principles"
categories: holistic
---

# Simplifying Down To First Principles
{: style="text-align: center"}

Written by Nick Otter.

# Contents 

- [**Introduction**](#introduction)<br>
- [**Piotr Kruckwoski**](#piotr-kruckowski)<br>

# Introduction

Identifying a resilient debugging strategy is endlessly useful. [This post](https://www.linkedin.com/pulse/simplify-down-first-principles-piotr-kruczkowski-/?trackingId=yT%2FvPodOT%2F2K88mMHkGhJA%3D%3D) from [Piotr Kruckowski](https://www.linkedin.com/in/piotr-kruczkowski/?lipi=urn%3Ali%3Apage%3Ad_flagship3_pulse_read%3B1a3smCl9Qv6%2Ftp3RkOmkTg%3D%3D) was found interesting. I will be hoping to expand on that. 

# Piotr Kruckowski

The first thing I do whenever faced with a problem or a complex system I do not understand, is simplifying it down to those things I understand well. I was in countless interactions where the person I was talking to, assumed that the topic of conversation is known and obvious to everyone involved. It was not obvious to me so I started asking questions.

* How does it work?
* Why is it implemented like that?
* What does it do?
* What the throughput and latency? 😎

In these questions I was trying to extract the essence from complexity. You can always extract the essence, the core of the idea from complexity of context. A known proponent of this way of thinking is Elon Musk.

~~~~
Well, I do think there’s a good framework for thinking. It is physics. You know, the sort of first principles reasoning. Generally I think there are — what I mean by that is, boil things down to their fundamental truths and reason up from there, as opposed to reasoning by analogy.
~~~~
~~~~
Through most of our life, we get through life by reasoning by analogy, which essentially means copying what other people do with slight variations.
~~~~
~~~~
Elon Musk
~~~~

Let me share with you an example situation where I was helping a fellow engineer understand a problem in their code.

*A: I have this big complicated mess of a monolith and I cannot understand what it does. Can you help?<br>
P: Why do you need to understand what it does?<br>
A: Because there is a bug in the image capture and logging thread.<br>
P: How does the bug manifest?<br>
A: Well it not actually a bug, rather a performance issue.<br>
P: How do you know that?<br>
A: The system is slowing down a lot when I turn on the 3D visualisation code, and it gets progressively slower, and then I get an image acquisition library error, that the image buffer is full.<br>
P: What does image buffer have to do with the 3D visualisation code?<br>
A: Nothing… which is why it’s so weird.<br>
P: OK, you said its a performance issue. Where did you get that idea from? Did you measure the performance? What is the difference? Can we graph it over time?*<br>

<u>We captured the performance data together. It showed a significant slowdown the moment visualisation was enabled, and a progressive slowdown until crash, with accompanying memory usage growth.</u>

As you can see from this conversation, there seems to be some connection between the 3D visualisation of data and the image acquisition functionality. These two functionalities depended on each other, but maybe the way how this dependency was designed was not the best. Let's continue.


*P: Why would the 3D visualisation slow down the acquisition? Are they in the same thread, the same loop? Is one blocking the other?<br>
A: Yes, they are in the same loop. But when I was testing the prototype of 3D visualisation I did not see it being that slow.<br>
P: What kind of data did you use for the test?<br>
A: A black image.<br>
P: So you didn’t actually test the visualisation, because there was nothing for it to visualise. It just checked the sanity, right? Lets try to run the same test with some more realistic data?*<br>

<u>Spending some time to run the test with realistic images.</u>

*A: Turns out that the visualisation code is 200x slower with real data.<br>
P: So it would never have a chance working together with the image acquisition code, if we want to hit 30 fps, right?<br>
A: Yeah, seems so… but what can we do? The images from acquisition feed directly into the visualisation. We cannot split them.<br>
P: Does the visualisation need to operate at the same speed as the acquisition? After all, this will be shown to the user as system health data. The visualisation is not a mission critical part of the system. The customer only cares that the image acquisition and logging is lossless and doesn’t drop any frames. They don’t care about visualisation speed.<br>
A: Does that mean we can run the visualisation slower than 60 fps?<br>
P: Sure seems like it. That’s one way to hit the performance requirements, but let's have a deeper look at the 3D visualisation… Why is it so slow in the first place… it not doing anything very heavy, and it’s hardware accelerated…*<br>

<u>After some more time spent looking into the 3D visualisation.</u>

*P: Yep, there’s your problem, there is a memory leak here. We create all these point objects in a loop, we show them, and then we never release them after the loop…<br>
A: So we should release them inside the loop, right?<br>
P: No, we only need to create them once. Then we only modify their coordinates in a loop. Afterwards we only need to release them once, at the end of visualisation.*<br>

<u>With the modification of the visualisation algorithm, the code was benchmarked again and worked 150x faster than the first version.</u>

*P: There were two ways of solving the problem, and both were justified at the time, with limited knowledge. But when we investigated, we figured out the algorithm was not implemented correctly. Still it might be a good idea to parallelise the two parts of the system, as it gives us a safety net if we have to process more data later.<br>
A: Thanks for your help! I will take care of it.*

The next steps in this story were fixing the algorithm, moving the things which can be done in parallel to be done in parallel, defining a clean interface between now parallel threads i.e. a pure data queue, and implementing both independently. 

Throughout the exercise we ensured that the unit testing includes real data and also measures basic performance metrics e.g. benchmarking.

We needed to simplify and decompose the system, and get to the basic principles of throughput, input data, latency, dependency, parallelism, SSD performance, CPU usage.
Function purity and data immutability are always your friends in exercises like the one above. Every single complicated software monolith, service, single module or class can be expressed as a set of actions, operating on some data, and producing other data.

If someone cannot express their solution as a composition of such abstracted blocks, it means they do not understand the system themselves.

You can also zoom in and out through many levels of abstraction using an approach like this. It does not require to drill down to the lowest level implementation of each functionality. Compositions of pure functions are pure functions themselves, operating on input data and producing output data. There is no layer at which performance starts or stops to matter, there is no layer at which a spaghetti of references breaks your brain, and prevents you from thinking about the basics. Assuming of course you are using a functional, data flow oriented languages like Clojure. I can imagine the exact same scenario requiring 10x the time to understand, if we were using a language like C++, C# or Java.

Microservice oriented architectures are a simple catalyst, enabling you to start thinking and breaking down solutions in this way. Microservices force designers to think about a system as a collection and composition of multiple, independent processes communicating with pure data. From this perspective everything that happens in one service is atomic. 

Now what is the underlying point of going through an exercise like this? 

It enables us to understand what the system really needs to do to process data from one form into another. This is what all software does. The secondary reason is understanding throughput. We can check if what we want to do is even feasible, frequently before coding starts. I had many situations where the original request, speed, resolution, amount of data, processing power, memory utilisation etc. were not matching the reality of the available hardware, but if we didn’t do a sanity check, we would only realise that too late.

A side benefit of asking deep questions to get to common understanding is that you are helping everyone around you, not just yourself. Many people might be as confused as you, but too shy, or scared, or vulnerable, or they might be accepting confusion as "standard ways of working". Do not accept artificial complexity, ask and clarify, and minimise noise.

---

Thanks. This was written by Nick Otter.
