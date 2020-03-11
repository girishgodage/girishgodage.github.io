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

![image info](/img/mobility/4/chat.png)

As you can see above, there are internal, external and layer rules that I've shared in here to build a really high performing web front end.

An important note – Remember when we spoke about architectures, we talked about separating different services into different concerns. Telegram actually started out as a hobby project, lack of these separations quickly made it difficult to maintain and add new features. In the dev world, we can define this as an architectural debt.

The proposed Angular based front end would suffer from a similar architectural debt if things aren't nicely separated in your API.

### Technical debt with Angular based chat apps

When things aren't done properly, especially with complex front end projects, things quickly go from manageable to a management hell.

As these projects move ahead and you focus on retention and user adoption, you would notice a lot of things that a lot of "To-dos" and "Fix laters" in your project management system.

While these were done in good faith, given the pace of how engineering generally works, what's left is often left.

This makes your product progressively less stable and less scalable with performance that can only be dreamt of without a major rewrite. Think of this as a mortgage with interest rates that almost double the moment you fail to make even a single payment.

### How to reduce Technical debt
I have almost spent a decade doing this for 100s of large scale projects, and a few general (but effective) principles that I see easily reducing technical debt here are:

* Improving test coverage
* Split large code files in to dedicated small files
* Keep migrating to the next most stable version
* Add continuous integration process

### Front end documentation debt

Technical debt doesn't happens in isolation, more often it is triggered by the lack of front end documentation. That's why I always recommending starting complex projects like developing a chat app with an up to date documentation in place.

I did talked about React as a potential technology piece to build a chat app, right?

Well, let's take a look at what this React based front end would look like.

### React based chat app front end architecture

React works fundamentally in different ways then most other web based frameworks. But, I am not going to talk about how React works here. If you wish to read more on the subject, you can read through my detailed blog post on React Performance.

This architecture as opposed to angular has a lot of benefits:

* Relatively faster
* Can achieve 60FPS without sweating even on mobile. 60FPS is what makes your user feel that the app is performant. Anything below that would make the app appear slow in performance
* Conceptually very simple

![image info](/img/mobility/4/chat1.png)

But if it was that easy, we all would have been building our chat apps on React. But, we all digress when it comes to selection on technology and framework.

If I talk specifically a really large and complex chat app, I would consider Angular given the fact that:

* There are tons of best practices for organization of large apps
* Huge support to automated testing and build generation

Again, it depends on your project and it's unique needs.

### How to make a messaging app – Features and Platform selection
Before moving to the real business of building app, you should have clear idea about what you want to build.

Platform selection plays and important part into app building journey, it can break or make your app.

Wrong platform selection cost you heavily in terms of resources and time in the long run.

So, before making our hands dirty lets take a look at the feasibility and implementation of each feature onto different platforms.

### Video Call
While Video call is something that's becoming extremely common these days with most messaging apps, implementing video chat functionality in my opinion requires a lot of consideration.

First of all, the way React native works, consider the following:

* If your app is only going to have video calling functionality run at a specific time, then you can consider using React native as an underlying technology for your app
* But if you have a notification, live emojis, or any other form of interaction, React native won't be able to deliver properly.
Why?

As we talked before, React native using something called a "bridge", this what helps your React native app's code to talk with the native components of mobile. And once when this communication Is started, this bridge won't allow anything else. For example, if this bridge is being used for video call, it won't allow even emoji's to go through.

Now you know when or when you shouldn't select React native.

But there are even more considerations.

For example,

Have you video-called someone using WhatsApp lately?

Have you noticed this highly interactive user interface that they've built?

![image info](/img/mobility/4/whatsappvideo.jpg)

While video calling was already difficult, what most startups and business ignore at the start is the whole idea of UI. Notice how much this UI is responsive to a user. This UI isn't just a random feature, this UI is about making something interactive, being able to read and reference messages and much more.

Imagine if your UX team says that you need to make your app more interactive, but sadly you picked up the wrong tech at the start. What would you do then? Would you scrap everything that you've built in React native just to make your app more interactive?

That's why you for video you should definitely start with native technologies. Even if you go with React native, you can fuse a native counterpart where React native code won't work.

Automatic contact sync
Contact sync is a continuous process and require a persistence connection with server.

Whenever you click on any of the chat application you must have came across a dialog box displaying Synchronising. It's basically synchronising your local data with server side data.

This process may also run in background without your knowledge (but with your consent), which can be highly battery draining and require high amount of processing power.

Contact sync is a basic feature and covered in depth in both native and react native. You can choose any of the platform based on your requirement.

Building Contact sync functionality is easy, be it native or React native. There are tons of examples available, and this is just not easy to implement, but also easy to implement with performance as well.

Typing Indicator in a chat app
To implement a typing indicator in your chat app, I usually recommend subscriber – publisher messaging pattern. Here a receiver would subscribe to the status of the sender, and publisher will publish sender's status.

Note that this typing indicators are heavy on your app's server. I've seen cases where they have occupied as much as 95% of all the messaging traffic, when actual messages only accounted for 5%. While implementing these keep a note of optimizing them very carefully, or have someone take care of it for you.

When we talk about building user interface of Typing indicators native apps based typing indicators significantly outperform the counterparts that we've built on top of React native. The main reason behind typing indicators working poorly on chat apps is because rendering this UI on React native causes issues.

![image info](/img/mobility/4/11.jpg)

Its native implementation like in android or iOS have inbuilt functionality and react listeners also provide its implementation in react native. But going with third party SDK's (Pusher, Jiver,  Applozic etc) in native provide you with performance improvement and ability to scale it better in future.

Native platforms have a few functionalities that further makes it easy to build these typing indicators for chat apps. But, if you are serious and intend to build an app for scale, you should also consider third Party solutions like Pusher as well.

Large File sharing
In chat application you require real time communication, which is, you wouldn't be reliant purely on client to decide when to fetch data from the server. Instead server will push the data down the stream to the client when it is available.

This can be achieved easily using socket.io.

Good news is, it's available for both native and react native platform.

If your only considerations are:

File size to be shared.
Speed of upload and download.
You are good to go with any- Native or React Native.

But in case of ease of implementation, React Native will pose a big challenge.

With React Native you need to hack around socket.io a bit because the client was originally built for the browser, and since the React Native runtime isn't a browser, it won't work right out of the box.

Well no one would like to do that heavy lifting and consume unnecessary resources.

So, Native is your bet to go.

In app camera
Camera integration is one of the trickiest task in app development. You have to take care of its responsiveness, number of frames/sec in a video and quality of image captured with their filters.

Let me take you to the real life challenge i faced when i was integrating this feature in React Native:

When i placed text with transparent background onto screen, the video lost the UI thread each time i move the scene. Resolution for this problem was available but with high cost of battery consumption.
On iOS, every time I tried to fix the height and width of image it's re-cropped and scaled from the original image. That was costly for large size images.
Well, these are just the overview of the problems I faced during different stages of development process, some other issue also revolve around the performance.

So, I would say the API React provide is still in early stage.

Let's wait for it to evolve.

Meanwhile stick to native to avoid all these hassle.

Camera API in Android and UIImagePickerController in iOS enable you to integrate system camera into your app.

While in case of react native, react-native-camera library, provide camera integration into your app.

Instagram type Stories feature
Stories are basically animations.

While playing around with react I took a challenge to implement stories feature with it.

To be honest, it was frustrating.

The animation used while swiping through stories looks like spinning a cube.

There are many tutorials available to showcase how to create a 3D cube using the power of CSS. But this property is not available with React Native altogether.

Result: 3D Animations are NOT possible in React Native.

"StoriesProgressView" provide us the ability to implement Status like WhatsApp or Stories likeInstagram into your app.

Using MobX with react native you can implement Status feature just like WhatsApp. But animations in React look like spinning a cube. There is also lack of different transform style with react which make stories look frustrating.

End to End Encryption
Security features have quit similar implementation in both react and native in terms of both resources and complexity. According to your requirement like availability of developers and level of implementation, you can choose any of them.

Signal API can be used to implement end to end encryption on native platform.

Inbuilt library "crypto-js" provide end to end encryption in React native. Signal API is also available with React Native for end to end encryption implementation.

Push Notification
Push notification are most frequently used feature in chat app. It will keep your users engaged with the app. Imagine you have done enough hard work on your app, launched it in the market, saw your user base growing.

But the very next day you wake up and notice user engagement dropped to few thousands from millions.

App is working fine but the issue with notifications can cause a great disaster, more so if your app is in MVP.

The most common issue we face with push notifications are:

Push notification stops when the app is in background.
You won't receive notifications of the durations when you were offline.
It may interfere with your phone UI.
Sometimes users stops receiving them when they upgrade their OS.
These issues can drastically drop down your user base.

Implementing them with React Native can be challenging considering its single threaded nature.

It performance can also differ with regards to android and iOS.

I myself stick to its native implementation to play it safe.

After all they are oxygen for my app.

In case of native, it can be implemented using Google Cloud Messaging(GCM) or Firebase Cloud Messaging(FCB).

I personally won't recommend you to go with GCM although its from Google. Popular apps such as WhatsApp and Facebook have their own protocols to implement.

If you have enough resources and decent budget to do so, well and good, go for it.

Else you can consider using FCM or Expo.

Search Widget
When i got the luxury to develop an app for my own, I was flooded with many emails that it doesn't contain search functionality, although it was there. When i talked to some of the users what i observed is:

"People seem to be more accustomed to the search bar found in other popular apps such as Facebook, Swarm and others. In those said apps, the search bar can be accessed directly through the Toolbar, meaning that the user can start a search right from the main screen."

As search bar was already there I decided to implement some transitions to it. I won't go into detailed story, but this is how I worked:

When user click on search widget

I faded the background content.
Expanded the toolbar and display the keyboard.
Fade the content back in.
With these changes my app looked like this:

![image info](/img/mobility/4/Searchbar2.gif)

This increased search query upto 30%.

As we already talked about, React Native is poor in implementing animations. This wouldn't be possible if i wouldn't have gone in native.

Considering performance, search functionality is similar in both Native and React, but in UI you have to accept upperhand for native.

### In-app Browser
When our app was in production, we started to push for React Native to make it more smooth and improve user experience. Webview in React provide the ability to integrate browser inside your app.

I would like to share our experience during the journey

* Webview executes each time a web page is loaded inside the app, which is time consuming. Another strange thing is it behaves differently in both android and iOS, your app can freeze while loading large sites having size more than 20mb.
Another question we were asked was "Did you get performance boost?"
Well honestly, the performance degraded significantly.

We dropped the idea altogether.

There are remedies available for the above disasters but performance difference in both iOS and Android will always be there.

Let's move on to its native implementation

In native implementation, you have two options to integrate it; Chrome Custom Tabs and WebView.

The WebView is good solution if you are using your own content inside the app. If your app directs the user outside your domain, we recommend you to use Chrome Custom Tabs for these reasons:

* You can save up to 700 ms when opening a link with the Custom Tabs by connecting to the service and pre-loading Chrome.
  
* The loading happens as a low priority process, meaning that it won't have any negative performance impact on your application, but will give a big performance boost when loading a link.

### Stickers and pop up stickers in line
In iOS, stickers for iMessage can be created using Xcode 8 and swift. When it comes to Android, stickers for GBoard can be created using Firebase indexing API.

You might have to integrate your React native app with the native (iOS/ Android) code to implement stickers in your Social media app. But the same require some heavy workaround which is not feasible solution.

### Read/unread message indicator
To implement read/unread indicator like whatsaap, use "DeliveryReceiptManager" and listener classes provided by android. While in iOS, observer of ChilChanged with Fireabse DB can be used.

React Native provide event listeners to implement read/unread message indicators.

### Bots
Integrating bot to your app has very high percentage of increasing user engagement.

These are not directly implemented like one on one user interaction, but uses Bot engine as additional component.

As we already talked about that multithreading is quite difficult in react native, or you can say not possible at all, Bot implementation will pose a big challenge ahead.

I have also faced some optimization challenges with bot implementation in react native.

This will increase your work overhead.

I would suggest you to go with its native implementation. You can choose any of the two options available, weather go with Firebase+Dialogflow or Microsoft's Bot framework.

Dialogflow is also available with React Native to implement chatbots in your app.

### Share location
Share location feature has become a instant hit when it was introduced by Whatsapp.

One of the biggest problem developers frequently face is the data usage while tracking someone's location.

It is widely accepted that data sharing uses huge battery but data usage is debatable. Different platform uses their own mechanism to track.

It can be implemented using MapKit and Core LocationIOS framwork in iOS, while Android provide location service API's. Its extremely easy and 100% optimized.

While in case of React native, we need to create a wrapper around Google map's SDK and consume it, if you app is going to use any other feature, consider this as a mildy difficult to tackle task. React Native uses high data and battery consumption increases. This may prove frustrating in long run.

Moreover, consuming the wrapper and processing it will also make tracking less precise.

### Shake and look around like in WeChat
Shake listeners are tough to implement in react rather than in native. I would recommend native for such implementation.

![image info](/img/mobility/4/Shake-in-wechat.jpg)

Android and iOS both provide shake listeners to implement shake feature in your app.

React native provide inbuilt library, react-native-shake-event, to implement the feature.

### Real time chat screen updates
WebSockets, both in Android and iOS provide real time connection to the server and update chat screen at real time.

WebSockets are also available for React to implement the feature.

Real time chat update can pose some UI challenges with react, while in case of native its easy and smooth implementation.

### Chat Heads
Chat heads are very convenient for multitasking as a user can work and chat at the same time.

That means if you are using the calculator app on your phone and if any new message arrives, you can open the conversation and replay the message without switching app.

Pretty cool!!!

These can be built in both iOS and Android. It requires the developer to play around with some system level permissions though.

Chat heads can be created on React native, but it requires heavy optimization as multiple chat heads means multiple views. That leads to hacking around the package called "react-motion".

Also, If you want them to stay when the app is closed, I am afraid that there is not some react native library at the moment (as far as I know) that can provide you that

### HIPAA compliance
HIPAA compliance can be achieved using Firebase on any native platform. Along with it artificial data redaction need to be implemented that deletes chat messages from Firestore as soon as the messages are delivered.

HIPAA compliance can be achieved using Firebase for React native apps.

### Offline persistence
In native apps, we create sync services to make sure that we can persist data locally

In react native we can archive offline persistence by using redux-persist library. You could accomplish similar things by using AsyncStorage directly, though it would be much more manual.

## Instagram Zoom-Draggable Photo Feature

Instagram Zoom-Draggable Photo Feature
I already build a prototype like this before.

So, directly jumping on to my results I would like to show how you can use it on Instagram.

1. Instagram consists of photos in a feed that you can scroll through.
2. Each photo will only be zoomable and draggable if there are two touches.
3. With two fingers pressed, we can zoom and drag the photo around.
4. When a touch is released it will animate back to the original size and position.
   
Now we are clear that we are gonna make this feature work the way Instagram does it.

Although Instagram implemented this on native platform, I took a UI challenge to implement it on React Native.

When building this prototype, I faced an issue that occasionally makes the app seem like it crashes without any error. It seemed to happen with very fast gestures. It would just freeze and stop responding to touches. Turns out the scrollable list component was taking control of the gesture while the drag was in progress.

So considering these challenges, which are hard to eliminate at this stage with React Native, i would prefer Native at present  to implement the feature.

Different libraries such as Zoomy and PhotoView are available in android to implement the feature. While in case of iOS UIPinchGestureRecognizer library works best for zooming capabilities.

This can be achieved in React using Animated API or PanResponder. However i won't consider you to waste your time with that, as i have already did for you.

### Animations
Android provide ValueAnimator to implement animations. While in case of iOS UIView animation API can be used.

react-native-motions library in react native help in integrating animations into your app.

But still its not that stable and lack some features and ease of animation implementation as provided in android or iOS.

### Chat Bubble
Creating a bubble around chat is a challenge. I have tried it on Native as well as React Native.

Making the arrows appear was harder than I thought it would be, especially when you consider different screen sizes, different platforms (iOS and android). It works differently on different platform.

So finally, you have to work differently for for each Android and iOS.

In android ListViews and RecyclerViews can be used to create a bubble around the message.

While in case of React Native Chat bubble around message can be created using renderBubble property of the GiftedChat.

### Photo Editing
Several third party SDK's such as Creative SDK and Aviray SDK support both iOS and Andrid for image editing. Android also provide android.media.effect library and GPUImage framework in iOS to add photo editing functionality into your app.

In react you can use gl-react library to implement the functionality.

### photo editing
### Cost of making a messaging App using a off-the-shelf Platforms

Great!

We had gone through step by step process of making a perfectly scalable messaging app.

Now, taking some business considerations in mind, it would be an obvious question to ask- How will it cost to make this app?

There are different SDK providers which provide end to end integration to implement as many functionalities as you would ask. But it depends upon your choice, considering your scalability requirement, performance, number of user base you want to serve and project budget.

If it was upon me, as i have worked on many chat application development from the UI stage to its delivery, building MVP with considerable scalability with pay as you go model will be best fit.

At initial stage we won't be directly jumping on to fight against the big wigs, such as Whatsapp, Line, Viber etc, in the market. It would be wise to start slow and build as you go.

To compare different solutions we will look at their Licensing cost, Hosting Cost, Setup and Integration cost and finally Customization cost.

We would be considering these providers in three scenario:

* **1.** An app that is starting up.
* **2.** An app that has 10K daily users.
* **3.** An app that has 100K daily users.

Here are some of the top SDK providers worth considering:

### Quickblox

![image info](/img/mobility/4/Quickblox.png)

Good News! They don't charge any Licensing fee.

Next, move on to Hosting. Our starter app would be fine at Starter Tier and as we move forward we will enter in Pro Tier and finally into Large Tier if we cross 500K different users per month.

The downside of Quickblox SDK is it is very basic, so you need to write whole messaging interface which can take around 2-3 months.

### PubnNub

![image info](/img/mobility/4/Pubnub.png)

Even they don't charge for Licensing but our Starter app would fit in Free tier with little difficulty.

For medium app Scale Tier will come into picture with $799. For popular app we would go for PRO which would require a requesting quote for custom pricing.

### Pusher

![image info](/img/mobility/4/Pusher-1.png)

While above providers provide messaging backend, Pusher offers real-time message communication. It provides you channel to send data between two users.

Pusher charges both on active connections and number of messages per day. Assuming a user spends 10 mins messaging per day, that means each concurrent connection can support 150 daily users. This means that free plan could support 15K daily users.

Our starter app could easily fit into free tier, medium app will run on $49 per month plan and large app on $499 plan.

But there is a caveat here. Since Pusher is a data delivery service, you have to create your own message interface and whole message protocol. This can take around  2-3 months.

## Conclusion
These are some of the specifics you'll need to develop and app like WhatsApp. WhatsApp may run the market but there are still space for your app to thrive, once you understand what particular feature your app hosts that WhatsApp doesn't. The difference will make your app stand out.

The process may look complicated if you lack tech know how. You will want a team that explores all possibilities and leave no stone unturned for your business and development need.



