---
title: Microservices Architectures - Event Driven Approach
date: 2019-10-19 20:17:00 Z
permalink: "/blog/introduction-to-event-driven-architectures-with-microservices"
categories:
- SpringBootMicroservices
tags:
- learning
summary:  In this article, we talk about event driven architectures, in the context of microservices architectures. We also discuss what are the advantages of using an event driven approach. 
image: "/img/microservices.png"
author: Girish Godage
layout: posts
prevurl: "/blog/12-factor-app-cloud-native-microservices-best-practices"
nexturl: "/blog/software-best-practices-building-a-vertical-slice"
discussion_id: 2019-10-19-Microservices19
---

### The Need For Event Driven Architecture

Microservices architectures have multiple small-sized microservices talking to each other. Here is one such architecture:
![image info](/images/Capture-060-02.png)

There are a set of common components - technical as well as infrastructure. Examples of technical components are Security and Logging components. Examples of infrastructure components include Naming Server, API Gateway and Centralized Logging. 

When implementing microservices, you would want to make them as event driven as possible. Why? Let's take an example

### Take A Use Case

Consider the use case of an online shopping application. Whenever there is an order, it is received by the order service. There are several actions that need to be carried out in order service:
- Update the stock
- Send out email and SMS
- Notify the packaging team.

#### Monolithic Approach

An initial approach could be to have a single monolith application to take care of all the functionality. 

#### Microservices Approach

Alternatively, these are fairly independent activities, and the entire application can be structured to have microservices for them, in a straightforward manner.

![image info](/images/Capture-060-03.png)

In this approach, you create an order event for the request coming in, and place it in the Queue. The rest of the individual services listen in to the queue for order events, and do the processing subsequently. 

#### How to choose?

> How do we make the decision which of the two application architectures to choose? 

The nature of your application decides this. 

If your system does not have a very high load at any given time, and also does not have any pressing scalability requirements, then you might want go with a monolith approach.

However, when your system is a large , handling millions of requests every day, and also has very stringent scalability requirements, you might be better off with the microservices approach. 

> An event driven architecture would be the one best suited to your needs. 

### Advantages Of Event Driven Architectures

#### Improved Flexibility And Maintainability

One of the most important needs of an application is maintainability. Ease of maintainability comes with proper **separation of concerns**. 

In our example above, the OrderService is only responsible for collecting the order and placing it in the queue. It does not worry about how it is going to be processed, who is going to process it, and so on. 

Similarly,  the StockService is only responsible for updating the stock, and the same with the other microservices. 

If there is a need for an additional processing step on an order, you write a new microservice to listen on the queue, and easily integrate it into the system. 

Such an architecture is clearly extensible, and also easily maintainable, due to separation of concerns. 

#### High Scalability

Let's consider an example : One fine day, a large volume of emails need to be sent out. 

In that case, you have the freedom to create a large number of instances of the EmailService, to handle additional load. 

A similar thing can be spoken about the other microservices in the mix, depending on the need of the hour. 

This is a clear advantage of this architecture, over using a single component to handle all the functionality. 

#### Improved Availability

Let's say one of the services listening for order events from the queue, such as the PackageService, goes down. 

In the first architecture approach of using a monolithic approach, any one functionality going down would mean the application cannot process orders any more.

In case of an event driven architecture, the PackageService going down would not prevent the OrderService from putting the order event into the Queue. The OrderService can notify the user of successful request receipt. 

A notification request would then be sent to the troubleshooting team about the PackageServer going down, and while it is being restored, the order event remains in the Queue. It can be processed by all the other services during this time, and when PAckageService comes back up, it can process the event as if nothing untoward has happened.

#### Good Reliability

Event driven architectures ensure a good standard of reliability for the system as a whole. However, the individual microservices can function with different levels of reliability. For example the StockService normally needs to ensure a high level of reliability, the EmailService and SMSService a medium level of reliability, and the PackageService - between low and medium levels of reliability.

#### Good Performance and Responsiveness

Note that in our example event driven architecture, all that the OrderService does is receive the order and place it on the queue, before acknowledging the user. The user does not need to wait till all the steps are performed on an order. This has high high responsiveness, and is seen by the user as high performance. 

### Summary

In this article, we had a look at event driven architectures. We saw that event driven architecture is ideal for implementing applications with high load, and strict scalability needs. 

We saw an example of an online shopping application that employs such an architecture. The advantages of using such an architecture include improved flexibility and maintainability, high scalability, imrpoved availability, good reliability and good performance. 

---
{% include microservices4.md %}

{% include blogslide.html %}

