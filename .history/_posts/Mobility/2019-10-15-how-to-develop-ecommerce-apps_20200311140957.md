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

We do see information on how load balancers, server capacity and storage that we have – but we don't exactly see how internal services are going to work with each other. So, that leads to a situation where we don't know how to ensure "Adding items to the cart" works all the time, especially during a high velocity sale.

**2. Database scalability and performance is not benchmark driven**

![image info](/img/mobility/3/ecommerce-app-database-scalabilty-and-performance-challenges.png)

In this diagram we see that we have Database clusters that are connected with our services. There would be situations in which a particular database is heavily occupied by search queries, but our other services also need it for processing transactions. How are we going to determine which service to prefer? How are we going to decide from millions of possible problems which service to prefer? How are we going to make sure that information delivery is fast enough under all loads?

There are no definition of any of these things.

**3. Resource and cost allocation of the system is unclear**

![image info](/img/mobility/3/eCommerce-app-architectural-problems-overall.png)

Simply put, **we don't know predictably how this system would perform at all.** While we do expect it to perform relatively well, but it wouldn't do so without having you bleed capital on unnecessary resources like costly servers, load balancers, etc without any empathy towards engineering best practices.

Let's say thousands of dollars spent on advertising and influencer marketing did brought in 100,000+ potential buyers to your eCommerce app. If you don't have a tech infrastructure that can scale reliably, what's the point of spending all of this capital?

### Benchmarks for Developing eCommerce Apps
From my experience of developing and scaling ecommerce apps, I will share a realistic scenario for a Black Friday sale. **If you are doing decent volumes with your eCommerce store, these are the numbers that you are most likely to face:**

* 200,000+ orders per day
* 20+ orders per second
* 2000+ product search requests per second
* More than 100M total server requests in a day

Now, this is a scale that most eCommerce apps have to develop keeping in mind.

**Your goal is to make sure that your entire system is and what capacity it is capable of serving.** And, now that we have these numbers with us, our thinking would shift from scaling entire stack to scaling different services (cart, product search, etc) to match these benchmarks.

We will now break down our app into different services each of which has its own goals to serve. This is called following a microservices approach.

### Using Domain Driven Design for Developing eCommerce Apps

Since eCommerce apps are extremely complex and they keep evolving, it makes sense for us to follow a domain driven design here. With a domain driven design, we have specific teams and stakeholders responsible for each services.

Based on domain driven design, let's evolve our eCommerce app's server code into microservices.

This is what the evolved architecture would look like

![image info](/img/mobility/3/Microservice-based-architecture-for-ecommerce-apps.png)

You'll notice that our code is now divided into a bunch of well defined microservices. The image below shows you the top 10 microsevices that you would absolutely need in order to build a high performing, scalable ecommerce app.

![image info](/img/mobility/3/Developign-eCommerce-app-with-a-domain-driven-design.png)

While, I am going to cover developing eCommerce microservices in another blog post, I walk you through why they are extremely important for eCommerce apps.

**Lets take a look at the Product search services** – It should be able to handle **100M+ requests** in a day. If we break down this microservice, you will find this service has:

**1.** **It's own parameters** for load balancers and scaling

**2.** **Well defined server RAM** and **Memory** consumption

**3.** **Has its own dedicated database** from where it can pull and show information. No other service can use this database

**4.** **Its own API** to communicate and respond to user search requests

**5.** Has a well defined boundary of what it can or can't do
   
Now, let's say if from a single server based service, you can at max only deliver product search to 1000 concurrent customers, **you easily know how many servers it would take to handle 10,000 concurrent product search requests** (1000 requests ~ 1 server, 10,000 requests ~ 10 servers). And thus you can reliably scale each of these services.

Another benefit of following a domain driven design is that the app becomes 100% testable. And, if you are planning to automate code deployment to your app, this would be extremely helpful.

All in all, this is what this architectural was actually benchmarked and tested for.

![image info](/img/mobility/3/developing-ecommerce-apps-with-microservices.png)

If you interested in reading about eCommerce frontend development of your app, here's a blog post that I wrote on [How to make an eCommerce site using ReactJS?]("/blog/build-ecommerce-app-reactjs")

### Scaling services for demand and performance

Now that we know what each service is capable of, individual stakeholders of each services can now scale these services up and down independent of entire ecosystem. This would also tell you what parts of your ecommerce app aren't good enough for business requirements. You can exclusively focus and improve those parts.

For example, if you see your implementation of ElasticSearch isn't good enough to deliver product search results in less than 200ms with a load of 1,000 concurrent users, you can look for alternative solutions. **You can test alternatives and see how they work and deliver results, without significant investment and without breaking anything else.**

## What really happened with John's empty cart?

Coming back to the empty cart problem that I shared at the start. Without well defined microservices, when John added an item to his cart, it was added to a geo-local database.

Our app assumed that John lives in this region and won't move out. **Without domain driven design, stakeholders, product managers and developers had no idea that this could actually happen** to them.

We usually build a golden record to handle such use cases. When we do that, we have a well defined and controlled boundary of data and John would be able to see this cart no matter where John lives.

I hope what you have learned so far improves upon your understanding to how to build a cloud backend for eCommerce apps.

If you are interested in evaluating Serverless tech, you should definitely checkout this blog on [building eCommerce apps using Serverless]("/blog/ecommerce-app-using-serverless")

## Developing your eCommerce Mobile Application
We will now start working towards building your understanding of ecommerce mobile apps. The way we will move forward now is by looking at the following aspects of ecommerce apps that are extremely critical to get right:

* Dynamic UI
* Building Product screens
* Elements of your app and how you should develop them
* Implementing reusability within your ecommerce app
Offline capabilities to improve app user experience

### Ecommerce apps with Dynamic UI
A consumer usually may or may not notice this, but as an ecommerce app developer you should know this. eCommerce apps undergo constant UI changes, some are a part of improving user experience, others are there simply because they are for a one time observable event like Black Friday.

Now, **simply pushing UI updates to your app each time a new event is near isn't possible** because:

**1.** *App Stores aren't very reliable in release management*, and approval of an updated app version could happen sooner or later.

**2.** *Asking users to update their apps for an event like Black Friday* or a UI intended for a Flash sale *would only result in user churn*

So, how would you solve this?

**One way to do that is to plan upfront and compile all app UI in a quarterly app update**. You can change this update frequency based on how frequently your users would be willing to update their app. But as a benchmark, don't go below less than 2 months.

Now, **when an app user updates their app, this is all what they are going to see.**

![image info](/img/mobility/3/ecommerce-app-code.png)

While, **this is what we are actually shipping**

![image info](/img/mobility/3/ecommerce-app-ui-updating-package.png)

