---
title: Microservice Architectures - Challenges with building Microservices
date: 2019-10-17 20:17:00 Z
categories:
- SpringBootMicroservices
tags:
- learning
image: https://girishgodage.github.io/img/microservices.png
author: Girish Godage
layout: posts
prevurl: https://girishgodage.github.io/blog/Microservices9
nexturl: https://girishgodage.github.io/blog/Microservices11
discussion_id: 2019-10-15-Microservices10
---

Microservices architectures are very popular today. In this article, we discuss the important challenges in developing a microservices architecture.

* Identifying Microservice Boundaries - Implementing Bounded Context
* Having Proper Configuration Management
* Implementing Technology for Dynamic Scale Up And Scale Down
* Ensuring High Visibility
* Ensuring High Fault Tolerance

 

### Identifying Microservice Boundaries - Implementing Bounded Context

We are aware that in a microservices architecture, instead of single big monolith application, we build a number of independent microservices talking with each other:

![image info](/images/Capture-066-02.png)

> An important question one faces while designing such a system is, how do we define the boundaries of individual microservices? 

How do you decide what one service does, and what it should not do? 

This is particularly difficult for a new application, as there may not be enough business knowledge to establish the right boundaries.

The task of deciding the boundaries of microservices is an evolving process. It is not something you necessarily get right the first time.

It often involves using domain driven design while playing around the functionality of the microservices. As you gain design knowledge, you need to put that knowledge back into the microservices architecture.

The best practices include following domain driven design.

### Having Proper Configuration Management

Configuration management poses quite a stiff challenge in implementing microservices.

Suppose that in our system, we have a few microservices, each of which has multiple instances in each environment:

![image info](/images/Capture-066-03.png)

In a slightly larger system, there could be 10 microservices with 5 environments, and 50 instances at the least. We are talking about tons of configuration  here! 

This will involve a lot of work for the operations team, just to maintain all these configuration.

Typically, centralized configuration helps you manage configuration.

### Implementing Technology for Dynamic Scale Up And Scale Down

For a microservices architecture, establishing the technology to do dynamic scaling of infrastructure, is a mountain to scale in itself.

The loads on different microservices could be different, at different instances of time:

![image info](/images/Capture-066-03.png)

At a particular point of time, you may need 2 instances of Microservice2. However at a later point in time, that need may go up to 4. Again, it could shoot up to 10 later. In essence, we need to bring up new instances of a microservices when they are in demand, and bring them down when no longer needed.

All this scaling needs to be done with dynamic load balancing. At the point of time shown in the diagram, the load from Microservice1 needs to be distributed among the 4 instances of Microservice2. Later, when there are 10 instances of Microservice2, there needs to be another load redistribution among those 10 instances.

Building an architecture for this involves a number of technical components - Naming Server, Load Balancers, Monitoring tools to identify whether to scale in or scale out.

### Ensuring High Visibility 

Ensuring visibility is one of the most important challenges that needs to be addressed.

![image info](/images/Capture-066-02.png)

If we say that the functionality of the above system needs to be distributed among 10 microservices, but there is a bug, how do you identify where the bug is? There is clearly a need for a centralized log, where we need to find out what happened with a specific request.

We also need to have effective monitoring across the microservices.

### Ensuring High Fault Tolerance

If not well designed, a microservices architecture can behave like a pack of cards. A microservice at the end of such a chain would be fundamental to the system behaving well.

Therefore, it is very important for you to have sufficient fault tolerance in place, in your system.

### Summary

In this article, we looked at the various challenges that need to be addressed, while designing a microservices architecture. We covered areas such as bounded context, configuration management, dynamic scale up and scale down, visibility and fault tolerance.

---
{% include microservices2.md %}

{% include blogslide.html %}

