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

![image info](/img/mobility/4/Chat-Architecture-3.png)

Now, let me walk you through why this architecture is great for growth and somewhat post growth stages.

##  NodeJS, AWS MQ and AutoScaling

Let's take a look at this tiny part for now.

![image info](/img/mobility/4/Chat-Architecture1.png)

This is the magic sauce of your app. Let me tell you why!

I have used NodeJS here because, right out of the box it can handle 100,000 concurrent users on a single core of your server. Most of our servers are 5-7 cores these days.

If you are familiar with WordPress plugins – adding/upgrading a NodeJS based app is exactly similar to that. You can build features or parts of your backend separately and assemble them together – that's how incredibly awesome NodeJS is!

> Even with simpler optimizations, you should be able to handle 1 Million users with a single server.

Let's get back to our setup again.

### Overcoming the limit of # of users per chat room

After working with a lot of chat apps, we realized that chat rooms has a problem, one that needs someone to solve it. If you look at Telegram, Whatsapp, Skype, Slack, etc you would find that you can't add users more than a specified limit.

So, I spent days around looking for ways to solve the problem. I came up 7 different ideas and did load testing on those solutions. Here's a result of load testing that I did on this solution using my propose architecture

I was extremely relieved when I found out that I now have a solution that can **host around 70k users in single chat room** without degrading the performance. I was **easily able to handle 80-200 messages per second** as well.

What's so special that makes this app extremely performant and scalable under heavy loads?

* **Regional availability** – I used Amazon's Multi AZ services for best regional availability
* **Message queues** –  I used AWS MQ to make sure that any information transferred from apps to server gets queued, parallelized and reliably stored without any performance bottlenecks
* **Auto scaling** – Added Load Balancer and autoscaling rules in AWS to make sure that if more computing power is need AWS automatically adds more server without need me to approve them
* **Separated business critical functionalities** and 3rd party functionalities when I wrote NodeJS based app here.
* **JSON serialization** – This may seem to small or technical, but optimized and reducing the size of the information that you are communicating to the server reduces a lot of load as well
* With chat apps, **push notifications are a real pain** if they aren't done right. Your app could be delivering 200 push notifications in a day, and separating it with it's own infrastructure will help you a lot.

## Database architecture for chat app scalability and performance

So, we spoke about faster message history retrieval and chat rooms before. But, if **I ask you go to on an excel sheet of 500 rows**(not sorted alphabetically), and find 3 names. How fast do you think you would be able to do it?

Updating and storing information on your database(DB) isn't magic either. Though, algorithms are much faster at information retrieval, but **with a messaging app, databases quickly become huge in size and complexity**. Making it very difficult to find, sort and update information. The algorithms below better highlight how information retrieval can get time consuming when you pick one way over another

![image info](/img/mobility/4/database-sorting-in-chat-apps.gif)

To better understanding DB level challenges, let's first take a look at the type of information that we usually store:

* Chat room information – Chat room name, who's in this chat room, etc
* Recent messages – These are messages that were just sent
* Archived messages – Old messages usually. But you can formally say that any message that isn't in the last 50 messages is an archived message.
  
Now, we also need a pipeline that can reliably push information to your database.

So, we have four different concerns when it comes to the database part of your app's architecture.

### Engineering considerations:

* A user would feel a poor user experience if he can't find the **last 50 messages** on his app
* A user would want to push information as fast to the server. **Real time chat** is the norm now!
* **Chat rooms would require their own datastore** as storing this information on the same database would be fatal for your app's performance and scalability
* **More than 100-200k messages would be published daily** if your app got more than 100k users.
  
It is quite common for chat apps to acquire users fast and scale. GoChat and GoSnap got their first 100k users in just a few days, organically!

Database setup for chat scalability and performance would include:

* Metadata database
* Hot chat history database
* Cold Chat history database
* Message Queue in front of cold chat history

**For the Metadata database**, we can use any key value store that has a rich data structure. You can even use Redis if used with RedisLabs for persistence.

**For Hot Chat database,** Redis can be used in a cluster or shared mode. You can also consider Aerospike, ScyallaDB or ElasticSearch as well.

**For Cold Chat data storage**, Risk is one of the popular choices to be used. But you can also use Cassandra/ScyllaDB, AeroSpike.

### **What performance can you expect from this setup?**

I can assure you that you can expect this work with no problems with:

* 15 Million messages/day
* 200 messages/sec

Good enough to support you, and simple enough to not cost you the earth to build it.

Architectures have types too!

* Event driven architecture + Simplicity + Iteratiblity
* Use 3rd party architectures like Firebase and Layer
* Decentralized Chat architectures
* Open source chat architectures
* Platform based architectures
* Enterprise grade architectures

What you read before was an Event driven architecture. We will take a look at the some of them in the sections that follow.

## Template Based Chat Architectures: Firebase and Layer

Chat apps built on top of Firebase is a great example of such architectures. In case if you don't know, think about Firebase as a set of preconfigured rules for building your apps where you only have to work on the front end, most of the app's backend logic would already be built. Remember how I tackled scalability problems earlier, with Firebase, you won't be needing to do that and they'll handle most of the things for you.

Here's what a messaging app architecture using Firebase looks like

![image info](/img/mobility/4/firebase.png)

That's it. Simple, efficient and fast

If you are looking for another platform that brings premade foundation to the table, take a look at Layer.io. They are similar to Firebase, but they also provide premade UI elements.

There was a time when folks at Secret were planning to build a real time chat solution. They started with more or less similar concerns that I've listed before. They were left with three choices:

* Use a 3rd party chat service like Layer
* Use a 3rd party websocket implementation like Pusher
* Build their own websocket based protocol on top of AWS

This is where I would expect you to take notes on technology selection. Secret's team quickly ended up rejecting Layer as a possibility, as they couldn't iterate or customize it enough. Plus they wanted complete control over their data as well.

Secret decided to go with Pusher to build a highly scalable websocket based system. Here's what their high level architecture looked like

![image info](/img/mobility/4/pusher.png)

This solution easily scaled well to over 1,000,000 concurrent connections. It was quick, it was fast and it was scalable.

When to go for a technology solution like Pusher, Firebase or Layer:

* If you are business heavy team that expects very low technology overhead, these 3rd party architectures are the best fit for you.
* If you are a technology team that just wants to launch a prototype faster, these 3rd party architecture are something that you might consider here.

Some precautions that need to be taken while evaluating these 3rd party platforms:

* In most of the cases, they control your data
* The system is often rigid and inflexible, makes it difficult to adopt to new custom changes
* Sometimes you would end up hacking the entire system just to make sure that you can get even a tiny custom feature
* Apart from what's already there, the system won't let you exceed benchmarks in terms of performance

About two years ago, I was working with this high growth team that was acquiring 100,000+ users every week. As a business heavy team they decided to go with Layer. At the start everything was great, except when the product manager decide to increase retention metrics by adding custom features. Layer proved to be rigid, and even adding basic features like "last seen" required a lot of work. We were able to handle this easily, but imagine an inexperienced developer who won't be able to figure of things and deliver them as fast as we did. That's where the real challenge starts with Layer. Other than that, if you have well defined specs that match what Layer can deliver, it's a great match!

### Decentralized Chat architectures

So far, what we have seen are something called a centralized chat architectures. Decentralized architectures are mainly based on top of Blockchains or Distributed Ledgers to support decentralized secure chat services.

A lot of the past work that I've done into decentralized chat architectures was extremely unique and it would be difficult to point out an architecture as generic as we did for a centralized chat service.

I will pick up here Matrix.org for building a B2B chat app on decentralized networks. Matrix as a solution would focus on:

* Group chat
* WebRTC Signalling
* Reducing Silos

Rather than being hosted on a centralized server, here's how Matrix based architecture really look

![image info](/img/mobility/4/Matrix-Architecture-1.png)

This architecture in general is a mesh of services. The really cool thing about this architecture is that the conversation history is distributed across these servers. There's no single server that owns the conversation.

A huge issue I feel that exists in messaging is the lack of open protocols and standards that could facilitate this communication. Matrix simplifies this communication by using WebRTC for communicating information.

While most of the overall architecture for an app that's built here is similar to what we have in centralized chat solutions, the difference comes in how decentralized chat nodes interact with each other. Take a look at the example below that shows two decentralized nodes (home servers) talking to each other

![image info](/img/mobility/4/How-data-flows-between-clients.png)

If this seems too confusing to you. Let me walk you through the practical sets of building a matrix app:

* Buy a domain name: like Slack.com
* Get a hosting(your own server) – This would start your services
* Using Linux Command line tools install Synapse on your server
* You now have a what we call a home server
* Write the other part of your app's logic – This would include everything right from how your app would look to how you are going to accept users in a chat room
* That's it
  
A more difficult part of building decentralized chat apps is the testing. As I said before, almost each and every decentralized chat app I've seen before is unique and that makes testing them extremely difficult. Load testing a decentralized app that was built from scratch to resist network spamming again is a tough nut to crack.

That's all from the backend architecture of your app. But there's front end as well, and there's a lot I've learned by failing at building web interfaces (front end) for chat apps.

### Front end architectures for building a messaging app

Without making you read another supposed book on the front end tech, I will simply walk you through two major technology frameworks that people use to build chat front end:

* Angular
* React

If I had to create a messaging app today on Angular, a high level architectural diagram of the client would look something like this:

If you are interested in deeper level analysis of an angular based front end whose miniature version you saw above, here's the detailed level architecture: