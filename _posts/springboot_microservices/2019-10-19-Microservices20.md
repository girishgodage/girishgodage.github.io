---
title: Microservice Best Practice - Why do you build a Vertical Slice?
date: 2019-10-19 20:17:00 Z
permalink: "/blog/software-best-practices-building-a-vertical-slice"
categories:
- SpringBootMicroservices
tags:
- learning
summary: In this article, we look at what is a vertical slice, and why we build it.
  We also discuss the best practices involved in building vertical slices.
image: "/img/microservices.png"
author: Girish Godage
layout: posts
prevurl: "/blog/introduction-to-event-driven-architectures-with-microservices"
nexturl: "/blog/messaging-queues-and-asynchronous-communication-in-microservices"
discussion_id: 2019-10-19-Microservices20
---

In this article, we look at what is a vertical slice, and why we build it. We also discuss the best practices involved in building vertical slices.

### What you will learn
- What is an vertical slice?
- When do you build a vertical slice?
- What are the advantages in building a vertical slice?
- What are the best practices in building a vertical slice?

### Need For A Vertical Slice

Let's say you want to start off writing a new microservice, or developing any new application. There are several technical risks that need to be addressed , such as deciding what framework to use, identifying the layers of the architecture, figuring out communication with other systems, and so on. 

You want to make sure you have great unit tests in place, and also have a good continuous integration and deployment system to process the builds.

How do we address these challenges?

### What Is A Vertical Slice?

A vertical slice involves selecting one or more of the complex or risky use cases of the systems, and implementing them first. 

Typically, this involves addressing all the risks listed above - selecting the right framework, organizing into layers, handling communication properly, having great tests in place, and following a CI/CD process. 

You will also deploy and test it to make sure all requirements are met.

### Benefits Of A Vertical Slice

A vertical slice will help you identify and solve technology risk faster. Once a vertical slice is built well, adhering to coding and other standards, it could also act as a reference point for other developers in your team.

### Best Practices

Lets quickly look at a few best practices around building vertical slices:

#### Ensure Static Analysis

Make sure that a tool to perform static analysis of your code is in place during the development of the vertical slice. Also make sure development adheres to all relevant best practices.

#### Ensure Continuous Integration

It is very important you have a continuous integration process in place, to take the vertical slice build through development, QA, staging and deployment.

#### Address Technical Training Issues

If the vertical slice is well implemented, it acts as a good reference point. This means that the development team can be trained using the vertical slice as a case study. 

### Challenges

It is very important that care be shown in selecting the use case for your vertical slice. Make sure that you choose a sufficiently complex use case, because you need to explore how the various technical risks will be addressed.

### Summary

In this article, we had a look at what are the technical risks involved in starting application development. We then saw how creating a vertical slice, using a complex use case, helps in exploring solutions to these risks, and acts as a reference point for the development team. We explored the best practices for creating a vertical slice, and looked at the challenges involved.  

---
{% include microservices4.md %}

{% include blogslide.html %}

