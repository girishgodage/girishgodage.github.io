---
title: Microservices Architecutres - Quick introduction to Cloud - Why, What and How?
date: 2019-10-17 20:17:00 Z
categories:
- SpringBootMicroservices
tags:
- learning
summary:  In this article, we focus our attention on the cloud. Why do we need the cloud?
image: /img/microservices.png
author: Girish Godage
layout: posts
prevurl: /blog/introduction-to-cloud
nexturl: /blog/introduction-to-spring-cloud
discussion_id: 2019-10-15-Microservices7
permalink:  /blog/introduction-to-cloud
---

In this article, we focus our attention on the cloud. Why do we need the cloud?

## You will learn
- What is Cloud?
- Why do we need the Cloud?
- How does cloud provide auto scaling?
- What are the advantages of Cloud?
- What are the challenges with Cloud?

### The Need For The Cloud

Loads on applications is not consistent throughout the year:
- Consider a startup company, which could have a potential to grow very fast.
- Imagine an online shopping portal, whose customer load varies throughout the year. 
- Other online business portals, such the insurance sector, which have peak load at a particular time of the year, and very little load during other times.

> If you provision your infrastructure to be able to handle the peak system lead, what would that infrastructure be doing the rest of the time? 

Most of it would just be sitting idle, watching the fun. 

That is exactly the problem cloud wants to solve.

Cloud intends to make it easy to provision infrastructure when you need, and just as easy to release it when you don't. This feature of the cloud has created a lot of buzz around it, over the last decade or so.  

### Building Applications For The Cloud

The thing is, you cannot take any application and just put it on the cloud. 

> A lot of things need to be right, in order to make an application cloud-enabled. They are known as **cloud-native applications**. 

There are best practices in place to develop such cloud-native applications, also known as the twelve-factor apps. We discuss those in a seperate article.

#### Advantages Of The Cloud

##### Auto Scaling and Agility

There is no longer a need to statically provision infrastructure to ensure availability for peak loads. You can do that on-demand.

##### Cost Reduction

There are significant cost reductions with using infrastructure from the cloud, because you only use what you need for the duration you need them.

##### Scalability And Elasticity

When applications are built well, they can auto scale based on the load.

##### Reliability 

Since your applications are cloud-enabled, they are designed to withstand certain kinds of outages. For example, applications with microservices architecture have fault-tolerance built into them. 

### Migrating Applications To The Cloud

It is not very straightforward to migrate your application to be cloud-enabled. 

The first step is to make them cloud-native. The next decision to make is choosing a platform to migrate the application to. You generally have choice of platforms to migrate to, such as Microsoft Azure, AWS and Google Cloud, among others.

### Best Practices

The best practices listed for making applications cloud-native are often termed as **12 factor apps**. These capture the experiences of Heroku, with developing cloud enabled applications. 

### Challenges With Developing Cloud Applications

#### Security

Security is a major concern, as the infrastructure is not what you own, and it can be dynamically acquired and released. 

#### Application Compatibility

Most applications are not compatible with the cloud. Applications have to enhanced to make them cloud native.

### Manifestations Of Cloud

#### IaaS

Infrastructure as a service is the first level of cloud. Cloud provider only provides the hardware needed to run applications.

#### PaaS 

Platform of service is the next level of cloud, where the cloud provider provides you a platform - an operating system or managed software (databases, programming languages, web application platforms) - on top of the hardware. 

#### SaaS

Software as a Service is the third level of cloud, where the cloud provider offers a software product on the cloud - like Google Docs.


### Summary

In this article, we had a good look at the cloud. We saw that a cloud helps ease the infrastructure management of online businesses by making provisioning dynamic. A cloud could be public, or private. Using a cloud has many advantages to offer, such as agility, cost reduction and reliability.


---
{% include microservices2.md %}

{% include blogslide.html %}