---
title: How to Make an eCommerce App
date: 2019-10-15 10:41:00 Z
permalink: "/blog/how-to-develop-ecommerce-apps"
categories:
- Mobility
tags:
- learning
summary: While all eCommerce apps differ by what they sell, they definitely face almost similar challenges when it comes to developing and maintaining them. After analyzing 150+ eCommerce apps, I wrote this guide on how to develop eCommerce applications. It walks you through everything you need to build a eCommerce app.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2010-10-15-how-to-develop-ecommerce-apps
---

# How to Make an eCommerce App

 While all eCommerce apps differ by what they sell, they definitely face almost similar challenges when it comes to developing and maintaining them. After analyzing 150+ eCommerce apps, I wrote this guide on how to develop eCommerce applications. It walks you through everything you need to build a eCommerce app.

 If you have to take lessons on developing eCommerce apps, you should take a look at these eCommerce apps

 ![image info](/img/mobility/3/developing-ecommerce-appl.png)

 While these apps differ by what they sell, they definitely face almost similar challenges when it comes to developing and maintaining them. **What starts as a simple app built on top of iOS, Android Java, couple of backend languages and databases, quickly evolves into a complicated technology** piece that can have as many as 15+ programming languages supporting it.

If you aren't careful enough while building eCommerce apps, you can end up making more mistakes than you can count. Consider the example below.

## John's empty cart problem

John adds an item to cart when he was in London. He then travels to New York and logs into his account to complete the purchase.

![image info](/img/mobility/3/how-not-to-develop-an-ecommece-app-part-1.png)


![image info](/img/mobility/3/how-not-to-develop-an-ecommece-app-part-2.png)

When he logs in, he sees an empty cart!

![image info](/img/mobility/3/how-not-to-develop-an-ecommece-app-part-3.png)

### What do you think happened here?

(The answer is in the architecture section of this blog post).

## Table of Contents

* How not to Develop eCommerce Apps
* A Better Way to Build eCommerce Apps
* Ecommerce App Scalability & Performance Challenges with this Architecture
* Benchmarks for Developing eCommerce Apps
* Using Domain Driven Design for developing eCommerce Apps
* Developing your eCommerce mobile application

### How not to Develop eCommerce Apps

There are no highs and lows of the scale at which an eCommerce app should perform. But, right from the start, don't just end up building something that looks like this.

![image info](/img/mobility/3/eCommerce-monolithic-architecture-for-apps.png)

There are probably hundreds of reasons on why we shouldn't build an eCommerce app like that. Some of the most commonly known reasons are:

**1. Pushing changes gets difficult** – No matter how small the change is, it gets difficult to push it across this app. It takes a lot of time for your development to modify this app for even tiniest bit of changes

**2.** At times it **isn't easy to figure out** what's causing your entire app to perform poorly
   
**3.** This app **won't scale** beyond a certain number of users
   
**4.** Even if your brute force scalability, it will come **at 10x the cost** of what it should be
   
**5.** The development team **cannot implement testing easily**. Even if you want to follow TDD, ATDD or BDD, you won't be able to, as the tests your developers would write won't be able to cover entire code base
   
**6.** If one of the functionality fails to work, it could **potentially take down the entire app**

**7.** **If a third party service breaks**, it could potentially break your entire app

**8.** **You will be forced to do** everything in one language, one version and pretty much will have to use the same libraries. You won't be able to leverage a diverse ecosystem

**9.** **Your database will break**, users will find that the flash sale discount that he just applied to a TV isn't valid anymore

**10.** **Search performance is poor** as we have to pull information from a single database that will grow extremely large as the ecommerce platform usage increases
    
**11.** All you rely on is a fixed number of servers that are **either overestimated or underestimated for the target delivery**. To handle increased loads, you need to manual add more servers and take them down when the load fluctuates. This is time consuming and extremely capital intensive

I can go on, but you get the idea, right?

## A Better Way to Build eCommerce Apps

![image info](/img/mobility/3/eCommerce-app-technical-architecture.png)

Now, with this type of an architectural layer, we improved the foundation of our system. When you compare this architecture with the previous system, the capabilities have been improved by:

**1. Servers and load handling capabilities**

If any ecommerce app was built on an architecture that we saw previously, we couldn't increase or decrease servers to match the server load fast enough. But, now we have a load balancer in place. It helps you define:

* how you prefer to handle the incoming loads
* how to add or remove servers when there's a fluctuating traffic
* how to set thresholds for costs

**2. You don't have to rely on a single language anymore**

***You can use Node.JS for API design and Python for server side programs – your choice!***

If you understand what performance you can get from a particular language or a server framework, you are no longer bound by limitations to only use a few of them.

**3. Can use different (types) databases to handle and store different information**

Unstructured data like emails, customer data can be stored in a NoSQL database. Whereas any structured information will now be stored into a MySQL database. Using specific databases for specific usage not only improves performance, but also reliability in case of eCommerce apps.

We call this a Polygot database approach, you can read more about it here https://en.wikipedia.org/wiki/Polyglot_persistence

**4. Partial separation of concerns**

We have third party services operating in a separate server(s) from our core service. As we don't control 3rd party services, they are a significant risk to our core infrastructure. If not well separated from our app infrastructure, their failures might cause your entire app to break down. Having third party services on a separate servers eliminates this risk.

**5. Faster search performance with ElasticSearch**

We now are using a **Search as a service – ElasticSearch**. It speeds up the way your system pulls information from your database using highly optimised indexing. It is worth noting here that search is one of the most frequently used part of an eCommerce app and has to perform really well.

**6. Cache service**

With a caching service, we look at the most frequently requested information, and create a data store (cache) for it separately. Now, when the same information is being requested by a user, it won't go to your database, but, would instead be delivered from the cache.

**But…**

While this might seems like a good architecture for a proof of concept or an MVP, but it won't work in cases where you are trying to achieve high growth or volume. eCommerce apps are supposed to follow a MVAP (Minimum Viable Awesome Product) philosophy right from the start.

Our diagram is fairly complex, let's break it down step by step to see where it lacks.

## Ecommerce App Scalability & Performance Challenges with this Architecture

**1. No Benchmarks of for code scalability and performance**

![image info](/img/mobility/3/eCommerce-app-architecture-and-scalability.png)






