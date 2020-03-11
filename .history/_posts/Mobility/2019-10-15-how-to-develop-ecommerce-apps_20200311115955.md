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





