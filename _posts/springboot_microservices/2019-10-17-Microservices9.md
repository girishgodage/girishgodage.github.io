---
title: Microservices Architectures - Advantages Of Microservices
date: 2019-10-17 20:17:00 Z
permalink: "/blog/microservice-architectures-advantages-of-microservices"
categories:
- SpringBootMicroservices
tags:
- learning
summary: Microservices architectures are very popular today. In this article, we discuss
  the three main advantages of having a microservices architecture.
image: "/img/microservices.png"
author: Girish Godage
layout: posts
prevurl: "/blog/introduction-to-spring-cloud"
nexturl: "/blog/microservice-architectures-challenges-with-microservices"
discussion_id: 2019-10-15-Microservices9
---

Microservices architectures are very popular today. In this article, we discuss the three main advantages of having a microservices architecture.
- New Technology And Process Adoption
- Dynamic Scaling Up And Down
- Faster Release Cycles
 
### Increased Technology Innovation due to New Technology And Process Adoption

A microservices architecture enables you to adopt new technology, and technical processes into your system.

![image info](/images/Capture-067-02.png)

Microservices usually communicate with each other using simple messages. Each of these can then be built using different technologies. 

For instance, it is possible to have Microservice1 written in Java, Microservice2 in NodeJS and Microservice3 in Kotlin.

Tomorrow, there may be a new language XYZ that is well suited for your application, and it could be used to write a new microservice for you.  

In a typical monolith application, we would have no such flexibility.  

> In addition to this, we can bring in new processes - for development, testing and deployment - for the new microservices that we create. 

### Reduced Costs due to Dynamic Scaling Up And Down

Consider a large online shopping application, such as Amazon. They do not have the same number of users or amount of load, throughout the year. Usually, there is a lot of activity with the application during the holiday season, and not much at other times of the year. If your microservices are cloud enabled, they can scale dynamically. 

This means there is no need to statically provision infrastructure to a fixed quantity for the entire year. 

> Infrastructure can be provisioned based on the load.

### Increased Business Innovation due to Faster Release Cycles

Since you are developing your application in smaller components, its much easier to release microservices as compared to monolith applications. This means you can bring new features, faster to market. That's a big advantage to have in the modern competitive business world.   

### Summary

In this article, we had a discussion about the advantages of microservices. We had a look at plus areas such as technology and process adoption, dynamic scaling and faster release cycles.  


---
{% include microservices2.md %}

{% include blogslide.html %}

