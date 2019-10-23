---
title: Microservices Architectures - Importance of Centralized Logging
date: 2019-10-19 20:17:00 Z
permalink: "/blog/introduction-to-centralized-logging-with-microservices"
categories:
- SpringBootMicroservices
tags:
- learning
summary: In this article, we explore the concept of centralized logging, in the context of microservices.
image: "/img/microservices.png"
author: Girish Godage
layout: posts
prevurl: "/blog/introduction-to-api-gateways-with-microservices"
nexturl: "/blog/introduction-to-auto-scaling-or-dynamic-scaling-in-cloud"
discussion_id: 2019-10-19-Microservices15
---

In this article, we explore the concept of centralized logging, with respect to microservices.

### You will learn
- What is centralized logging?
- Why do we need centralized logging?
- Why are microservices difficult to debug?

### The Need For Visibility

In a microservices architecture, there are a number of small microservices talking to each other:

![image info](/images/Capture-057-02.png)

In the above example, let's assume there is a problem with Microservice5, due to which Microservice1 throws an error. 

How does a developer debug the problem?

He would like to know the details of what's happening in every microservice from Microservice1 through Microservice5. From such a trace, it should be possible to identify that something went wrong at Microservice5.

The more you break things down into smaller microservices, the more visibility you need into what's going on in the background. Otherwise, a lot of time and effort needs to be spent in debugging problems. 

One of the popular ways to improve visibility is by using **centralized logging**. 

### Centralized Logging Using Log Streams

Using Log Streams is one way to implement centralized logging. The common way to implement it is to stream microservice logs to a common queue. Distributed logging server listens to the queue and acts as log store. It provides search capabilities to search the trace.

### Popular Implementations

Some of the popular implementations include 
- the ELK stack (Elastic Search, Logstash and Kibana) for Centralized Logging
- Zipkin, Open Tracing API And Zaeger for Distributed Tracing

### Summary

In this article, we had a look at centralized logging. We saw that there is a need for high visibility in microservices architecture. Centralized logging provides visibility for better debugging of problems. Using log streams is one way of implementing centralized logging. 

---
{% include microservices3.md %}

{% include blogslide.html %}

