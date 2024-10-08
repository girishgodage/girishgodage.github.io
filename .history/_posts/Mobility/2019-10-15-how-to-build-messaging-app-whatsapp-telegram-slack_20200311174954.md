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

If this solution was built by someone that didn't had any experience, they would've failed. But GoSnap's founder applied a ton of easy to do (but smart) optimizations. **He made certain optimizations in his architectural implementation that are worth noting:**

* **Applied resizing** before uploading an image to databases
* **Separating Snaps into different buckets** with newest, most liked, popular, etc as the criteria. That way he could extract relevant information without apply complicated request to his database. Any information that was request was already prepared, that improved performance to a huge extent for GoSnap
* **He was using MongoDB for GoSnap**, and there's a library called Mongoose that was being used for this app. He **stripped it away and used simpler functions** instead. This reduced load.

***Note**: Optimizing your app for scalability and performance is always an ongoing process. Removing Mongoose ORM that I've listed above was done only after the third day when the founder saw that the server loads went as high as 90-95% because of Mongoose ORM. The server load after this optimization went down to 5-10%, just by removing a few*

Now, imagine how many people miss this opportunity when they are creating a chat app!!

Now coming back to these two apps, this isn't a 100% fair comparison. Chat apps are a different kind of a monster, and they have to be treated like one. GoSnap wasn't a super complex app, but my point was to show you how having the right information can make or break you chat app.

GoChat actually was an extremely complicated product that had:

* Many to many chats
* A lot more messages
* A lot more information write requests to database
* A lot more operations
* Time dependent infrastructures

But this complexity wasn't paid attention to when the project was started. Subsequently, GoSnap's founder had to face weeks and weeks where the app wasn't able to work.

These two apps nicely illustrate with opportunities and threats your app would face. We will now get into details of how to architect your chat app for so that it is ready to handle millions of users with scalability and performance.

## Architecture of Messaging Apps
We talked about simplicity before, let's put it in context to your messaging app now.

A good starting point is that you are just starting and your customer base is small or is going to smaller for sometime. You aren't expecting a billion messages to be exchanged on your chat platform.

### Challenges that should shape your messaging app's backend?
So, let's build a list of challenges that should shape your initial architecture at this stage:

* Future growth – evolution of app
* Able handle 10k concurrent users
* Real time communication
* Get it built in 3-6 months
* Chat rooms should have enough size and capacity
* Searching across several channels simultaneously
* Scale when load is suddenly increased
* Faster retrieval of message history
* Be able to share messages with multimedia support
* These are some of the most common business challenges that we face when we are developing a MVP for chat apps.

Here's what your architectural foundation should look like
