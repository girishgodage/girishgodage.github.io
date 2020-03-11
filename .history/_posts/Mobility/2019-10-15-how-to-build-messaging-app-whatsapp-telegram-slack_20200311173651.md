---
title: How to Make a Messaging App like WhatsApp, Telegram, Slack
date: 2019-10-15 12:41:00 Z
permalink: "/blog/how-to-build-messaging-app-whatsapp-telegram-slack"
categories:
- Mobility
tags:
- learning
summary: How to make a messaging app that scales more than 10 Million users. The blog covers architecture and advanced features of a messaging app in detail. The goal here is to build a highly scalable chat app in less than 3-4 months.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2010-10-15-how-to-build-messaging-app-whatsapp-telegram-slack
---

# How to Make a Messaging App like WhatsApp, Telegram, Slack

 How to make a messaging app that scales more than 10 Million users. The blog covers architecture and advanced features of a messaging app in detail. The goal here is to build a highly scalable chat app in less than 3-4 months.

 ![image info](/img/mobility/4/cover-image1.png)

 If anyone asks me about the type of apps that I absolutely love working on, developing chat and messaging apps would top my list.

They are inherently so challenging, so complex, and at the same time so simple, that it could drastically change one's perspective on limits of **software engineering**.

I've worked with more than 10 chat apps as a part of our application development services where I was lucky enough to technically scale them beyond 10M users in a very short period of time. With this blog post, I am going to exactly show you how to make a messaging app.

## Table of Contents

* Story of two contrasting chat apps
* Architecture of Messaging Apps
* Template based chat architectures: Firebase and Layer
* Decentralized Chat Architectures
* Features of a Messaging App
* Advanced Visual Features like Animations and Chat Bubble
* Cost of building a chat application using a off-the-shelf platforms


**The goal here is to build a highly scalable chat app in less than 3-4 months.**

I would be taking you to step by step walkthrough for building an app that will reach more than 500000 users in just 5 days on less than $100/month server.

Before I get deep into the tech part of building chat apps, I will walk you through two chat apps first: GoChat and GoSnap.

### Story of two chat apps
**How to make a messaging app** - GOchat and GOsnap 
When Pokemon Go was released, we saw two chat apps that came to life along with it: GoSnap and GoChat.

Here's where things get pretty interesting. Both apps were nearly serving the same goal, almost identical number of users. But, if you look at the server costs, you would find:

* GoSnap's server costs: $100/month
* GoChat's server costs: $4000-6000/month

**GoSnap was serving around 1000** concurrent users, had 500k unique app users and 150k-200k snaps and messages were being uploaded daily. 100 requests per second were made to app's database daily.

Now, **GoChat on the other hand was serving 600 requests/second** with a little over **2 Million users in their database**.

At this point one might wonder why one app could easily serve 500k users with $100 and the other one could only serve 2 Million users and needed $4000-6000/month?

### Experience beats inexperience when it comes to apps

When it came to building a messaging app, GoSnap's founder went for simplicity, right from the programming languages, frameworks and the architecture he selected.

This is what his architecture looked like at a very high level

![image info](/img/mobility/4/Go-Snap-Architecture-2.png)

Looks nothing complicated, right?

That's in fact a good sign of an engineer and an architect. Rather than making things complicated they make it simpler.

