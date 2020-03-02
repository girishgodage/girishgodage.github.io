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

### Why do we need Containerization?
Containerization is needed because it intends to solve an important problem: to ensure that a software runs correctly when it is moved from one computing environment to another.  Containerization an application also enables different teams to work on different parts of the application independently, as far as there is no major change in the way how they interact with each other. As a result, it is easier to develop a software and testing it faster for all the possible errors.

In a world such as agile, DevOps, this has become more critical than ever. Containers simply make it simple for developers to know that their software will run without worrying about where it will be deployed. They also enable that we have commonly known as ‘microservices’.

Following are the items that are usually bundled in a container:

* Application
* Dependencies
* Libraries
* Binaries
* Configuration files

To manage all these containers, you are required to have another set of specialized software like Docker Swarm, Kubernetes, etc. This software helps you orchestrate these containers so that you can push them out to different machines while making sure that they run.

The Image attached below represents how Containerization works:

![image info](/img/awscloud/6/Artboard-6.png)

Now that we now why do we need serverless and containers, let’s have a feature by feature comparison of Serverless vs Containers so as to get a proper idea about each deployment option:

#### #1. Longevity Limitations in Containers vs Serverless
**Serverless**: As you may know, functions live very ‘short’. Here, short can be defined as 5 minutes or less. Functions are ephemeral, means, the container running the function may live for only once and die after its execution.

Shorter lifespan is one of the caveats of functions but it also provides agility which gives devs freedom and flexibility to push apps into the production which are easy to scale.

**Containers**: This is not the case with containers. Containers are always running and spinning and they do not die once their execution is done. This empowers them to leverage the benefit of caching, about which we will discuss later, but at the same time, scaling is not instantaneous.

#### #2. State persistency
**Serverless**: As discussed in the above point, functions are ephemeral or ‘short-lived’ which in turn, makes them stateless. The more stateless your functions are, the more ways there are to put them together and build something powerful.

The power of stateless computing lies in the ability to empower developers to write powerful, reusable functions and combine them. But it comes with the downside of caching. Since functions are stateless, you cannot cache anything for the further use, as a result, it faces high latency about which we will discuss in next point.

**Containers**: With containers, you can leverage the benefits of caching. To allow the data to be stored even when the containers are terminated, you’ll need to use a storage mechanism that will manage the data outside of the container. Anyways, why caching is so important?

If the objects on the object file that container is about to produce is same as the previous builds, reusing a cache from the previous result is a great time saver in terms of computing. This enables the building of new containers extremely fast.

#### #3. Latency & Startup Time
**Serverless**: Since functions are stateless, caching is unavailable and they don’t have copies of your functions running on standby, it results in higher invocation time. Functions stay warm, or your code is running while getting a command to be executed, for 15 minutes and die, hence, if you call it after the specified time, it will be a cold start.

As a result, you may face latency issues especially when there are concurrent users. To tackle this, you can add the following code to it to keep it warm all the time as mentioned in our previous blog: Serverless Performance: Challenges and Best Practices.

![image info](/img/awscloud/6/serverless.png)

However, this is temporary and can be used while you’re dealing with fewer functions. If you’re using it for all the functions, soon there will be a lot of dummy functions and then you won’t be able to manage it properly. However, the number of the function is quite small, it makes sense to use functions rather than spinning a whole container.

**Containers**: Considering the pre-serverless era of containers, these are always sitting there and all you have to do is send an HTTPS request to it and you get an instant response and low latency time. With caching at the advantage, containers can be spun fast with none of the files to be created again, as a reference to them is sufficient to locate and reuse the already built structures.

#### # Scalability in Serverless and Containers
**Serverless**: In serverless architecture, the backend of an application automatically and inherently scales to meet demand of the desired scalability. Moreover, Serverless Computing can be compared to the functioning manner of a water supply: your supply provider turn the tap on and your consumers can acquire as much water they need at any time. They only pay for what they use. This concept is far more scalable than the attempt to buy water one bucket, or shipping one container at at time.

**Containers**: While using a container-based architecture, it’s the job of a developer to determine the number of containers to be deployed in advance when it comes to scale the application according to the desired needs. Moreover, with the increased demand a shipping company would try to ship more containers to the destination to meet that demand. But this won’t be much scalable either when demand of consumers exceeds the shipping company’s expectation.

#### #4. Portability & Migration

**Serverless**: Let’s assume you’re already using many different services of AWS. If this is the case, then opting for Lambda functions would be extremely easy as it facilitates fast and accessible integration with its other services.

Even if it’s not the case and you fear vendor lock-in, what you can do is to make sure all API endpoints and URLs used in your code get mapped through a domain/change via DNS is under your control.

This gives you an option of cutting off a particular service or redirecting them to a different endpoint of your choice (eg. an another BaaS provider). This is better than hard coding your code in an endpoint which is not under your control or is unalterable.

However, there are many FaaS providers and your concern for vendor lock-in is quite obvious. In case of Lambda, if it isn’t meeting your region specific requirements, here is what you can do. All Lambda handler code should be isolated and extremely thin shims to logic that is locked up in other modules/classes.

This increases reusability and in the event that a refactor is necessary to move out of Lambda, makes that work much easier and straightforward. This also facilitates unit testing. An example of a thin Lambda handler:

![image info](/img/awscloud/6/code.png)




