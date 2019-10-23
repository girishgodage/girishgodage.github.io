---
title: Microservices Architectures - Introduction to API Gateway
date: 2019-10-19 20:17:00 Z
permalink: "/blog/introduction-to-api-gateways-with-microservices"
categories:
- SpringBootMicroservices
tags:
- learning
summary: In this article, we look at what an API Gateway is, in the context of microservices.
image: "/img/microservices.png"
author: Girish Godage
layout: posts
prevurl: "/blog/introduction-to-centralized-configuration-with-spring-cloud-config-server"
nexturl: "/blog/introduction-to-centralized-logging-with-microservices"
discussion_id: 2019-10-19-Microservices14
---

In this article, we look at what an API Gateway is, in the context of microservices.

## You will learn
- What is an API Gateway?
- Why do we need API Gateways?
- How does an API Gateway Work?

### Handling Cross Cutting Concerns

Whenever we design and develop a large software application, we make use of  a **layered architecture**. For instance, in a web application, it is quite common to see an architecture similar to the following:

![image info](/images/Capture-02-01.png)

Here, we see that the application is organized into a Web layer, a Business layer, and a Data layer. 

In a layered architecture, there are specific parts that are common to all these different layers. Such parts include:
* Logging
* Security
* Performance
* Auditing

All these features are applicable across layers, hence it makes sense to implement them in a common way. 

Aspect Oriented programming is a well established way of handling these concerns. Use of constructs such as filters and interceptors is common while implementing them.

### The Need For API Gateway

When we talk about a microservices architecture, we deal with multiple microservices talking to each other:
![image info](/images/Capture-059-02.png)

Where do you implement all the features that are common across microservices?
- authentication
- logging
- auditing
- rate limiting

That's where the API Gateway comes into the picture.

### How does an API Gateway Work?

In microservices, we route all requests - both internal and external - through API Gateways. We can implement all the common features like authentication, logging, auditing, and rate limiting in the API Gateway. 

For example, you may not want Microservice3 to be called more than 10 times by a particular client. You could do that as part of rate limiting in the API gateway.

You can implement the common features across microservices in the API gateway. A popular API Gateway implementation is Zuul API Gateway.

### Summary

Just like AOP handles cross cutting concerns in standalone applications, the API gateway manages common features for microservices in an enterprise. 

---
{% include microservices3.md %}

{% include blogslide.html %}

