---
title: Serverless vs Containers A Face-off between your Application Deployment Options
date: 2020-01-15 10:41:00 Z
permalink: "/blog/serverless-vs-containers"
categories:
- AWSCloud
tags:
- learning
summary: Like most of the existing cloud application development trends, Serverless
  and Containers have been widely spoken-of and discussed among developers. Some feel
  that Serverless originated as an alternative to containerization and some feel serverless
  can be used with containers for application deployment. The difference between Serverless
  and Containerization is not limited to performance, but also applicable for longevity
  limitations, application scalability, time-of-deployment etc.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-15-serverless-vs-containers
---

## Serverless-vs-containers

  Like most of the existing cloud application development trends, Serverless and Containers have been widely spoken-of and discussed among developers. Some feel that Serverless originated as an alternative to containerization and some feel serverless can be used with containers for application deployment.
  
  The difference between Serverless and Containerization is not limited to performance, but also applicable for longevity limitations, application scalability, time-of-deployment etc.

 ![image info](/img/awscloud/6/container-function-1.png)

 It’s 2019 and like most of the ongoing trends of devops, going serverless or using containers for application deployment seem to be a common point of debate among all new kids (pun intended!) on the block right now! And we still feel it’s not going to end anytime soon.

However, Is the Serverless vs Container comparison popular just because some cloud enthusiasts feel that serverless computing is a replacement for containers? OR Is it because some people feel that serverless is just another technology that can be used inside containers? 

In this article, we’ll compare both these technologies and try to find out which one (if any!) is better. What are the common and distinct qualities? Which one is suitable for what application use case? But first, let’s start with some background.

### Why do we need Serverless computing?
Over the past few years, we used to deploy our applications over big servers where the overall responsibility for managing or provisioning the resources is upon us. There are few issues with it:

* Unnecessary charges for keeping the server up even when we are not consuming any resources
* Overall responsibility for maintenance and uptime of the server is upon us
* We are also responsible for applying the appropriate security updates to the server.
* As our usage scales we need to manage scaling up our server as well. And as a result manage scaling it down when we don’t have as much usage.
  
With so many issues (like the ones mentioned above), it’s a lot of work to handle for smaller companies and individuals. Moreover, as a result this impacts the overall Time to market and cost of delivery which is otherwise the most important factors in **Custom software development**.

This is where “Serverless computing” comes into play. With Serverless computing, you are provided with an execution model where your cloud provider (AWS, Azure or Google cloud) is executing a piece of code by dynamically allocating the resources. This only charge you for the amount of resources that are used to run the application code. That’s a lot of cost cutting when you compare the *compute cost* with traditional servers. This way, your overall experience of computing is “serverless” (as the cost of managing server resources is less) but the infrastructure is not. If you want to know A-Z of serverless architecture, [*read our Comprehensive Guide to Serverless Architecture*](blog/serverless-architecture-guide).