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

2. At times it **isn't easy to figure out** what's causing your entire app to perform poorly
   
3. This app **won't scale** beyond a certain number of users
   
4. Even if your brute force scalability, it will come **at 10x the cost** of what it should be
   
5. The development team cannot implement testing easily. Even if you want to follow TDD, ATDD or BDD, you won't be able to, as the tests your developers would write won't be able to cover entire code base
   
6. If one of the functionality fails to work, it could potentially take down the entire app
7. If a third party service breaks, it could potentially break your entire app
8. You will be forced to do everything in one language, one version and pretty much will have to use the same libraries. You won't be able to leverage a diverse ecosystem
9. Your database will break, users will find that the flash sale discount that he just applied to a TV isn't valid anymore
10. Search performance is poor as we have to pull information from a single database that will grow extremely large as the ecommerce platform usage increases
11. All you rely on is a fixed number of servers that are either overestimated or underestimated for the target delivery. To handle increased loads, you need to manual add more servers and take them down when the load fluctuates. This is time consuming and extremely capital intensive

I can go on, but you get the idea, right?





