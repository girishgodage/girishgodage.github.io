---
title: Serverless vs Containers A Face-off between your Application Deployment Options
date: 2020-01-15 10:41:00 Z
permalink: "/blog/serverlessVScontainers"
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
discussion_id: 2020-01-15-serverlessVScontainers
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

Speaking of migration, it is still hazy how to fit FaaS into the current DevOps framework. It might happen that your organisation will write hundreds of function and after a time, nobody knows what functionalities were included in which functions and how many of them are still in use.

**Containers**: If you’re opting for container-based microservices architecture, these provide great portability. The time and efforts to move the code from developer’s laptop to your internal datacenter or out to different cloud providers are minimal.

With the immense stress on innovation and time to market are getting lesser and lesser, opting for microservices empowers you to spin up new versions of your application.

Hence microservices sounds like a good option to get started if you’re moving from monoliths due to ease in migration and various option for technology stack for multiple containers.

However, running a container on a cloud platform has a huge amount of interdependencies. Such as upgrades need to be planned together which includes container hosts, container images, container engine and container orchestration.

For some legacy applications that you want to shift to microservices, ‘containerizing’ them might be an easy and cheap option than to re-architecting the whole application into functions.

#5. Development Environments & Language support for Serverless and Containers
Serverless: The language supported by popular FaaS providers are quite limited which mainly includes Node.js, Python, Java, C# and Go (in case of AWS Lambda).

Containers: While containers empowers you with heterogeneous development environments and you can work on any technology stack you want. It might not sound like a huge benefit since developers these days are well-versed with multiple languages, but it is!

When you’ll be hiring for your new project, you won’t be required to take language into considerations for microservice architecture. Since microservices are independently deployable and scalable with each service providing a firm module boundary, services can be written in the language of any choice and can be managed by different teams.

#### #6. System control
**Serverless**: Dealing with functions such as AWS Lambda doesn’t come off as a hard task as it eliminates the infrastructure complexity, as a result, you can focus more on developing your product and business outcomes. It significantly reduces the time to market which is not the case with containers. However, there is a long list of things that should not be done while using AWS Lambda.

**Containers**: Plus, management of cluster configuration is a serious challenge as it prerequisites solid background in container technology. Talking about the control, microservices are comparatively easy to handle. Plus, orchestration framework like Kubernetes accelerates your governance and control over the architecture.

Container-based microservice architecture imparts you full control over the individual as well as the whole system. This enables you to set policies, allocate and manage resources. Plus, having fine grain control over the security and migration services.

With the full control over the container system comes the ability to see inside and outside of the containers. This allows comprehensive and effective testing & debugging using multiple environments and extensive resources. Whereas actual implementation and local testing of functions aren’t possible and hence it is hard to guess the performance issues.

#### #7. Resource heavy processing in Serverless and Containers
Let’s take an example of AWS Lambda, if the function is taking more than 5 minutes, you’ll be mandated to dissect these tasks into smaller ones, plus, these are not the only limitations while working with them.

You can allocate the maximum of 1.5 GB of RAM for executing a single function, your deployment package should not exceed the maximum size of 50 MB. Whereas with containers, you can allocate the computing resources as per your application requirements.

#### #8 Testing in Serverless vs Containers
**Serverless**: Testing is difficult in serverless based web applications as it often becomes hard for developers to replicate the backend environment on an local environment.

**Containers**: Since containers run on the same platform where they are deployed, it’s relatively simple to test a container-based application before deploying it to the production.

#### #9. Maintenance in Containers and Serverless
**Serverless**: Maintenance in Serverless based applications is much easier than you can ever imagine. Since your serverless vendor such as AWS lambda takes care of all sort of things such as management and software updates to the server, overall maintenance is less.

**Containers**: Unlike Serverless – with which a developer don’t have to think about maintenance – it’s a developer job to to manage and update each container he deploy.

#### #10. Cost comparison in Serverless and Containers
**Serverless**: As already mentioned above, using Serverless functions such as AWS lambda for application deployment prevents you for bearing unnecessary expenses of resources because application code does not run unless it is called. Instead, you will be charged for the server capacity that your application is going to use, anyways. If you want to have a clear overview regarding *AWS lambda pricing*, we’ve already covered that in our previous blog post.

**Containers**: Containers are constantly running, and therefore cloud providers have to charge for the server space even if no one is using the application at the time.

#### #11. Time of deployment difference in Containers vs Serverless
**Serverless**: Since serverless functions are smaller than container microservices and they do not come bundled with system dependencies, it takes only milliseconds to deploy an application. Moreover, Serverless applications goes live as soon as the code is deployed.

**Containers**: Although, Containers take longer to setup at the initial stages of development but once configured they only take few seconds to deploy.

## When to Use Serverless? : Serverless Use 

![image info](/img/awscloud/6/what-function-can-do-better.png)

*Serverless Computing is perfect fit for the following use-cases:*

* If your traffic pattern changes automatically. Not only it will be handled automatically, it will even shut down when there is no traffic at all.
* If you are worried about the cost of maintenance of servers and the resources your application consume, serverless will be great fit for such use-case
* If you don’t want to spend much time in thinking where your code is running and how!
* Serverless websites and applications can be written and deployed without handling the work of setting up infrastructure. As such, it is possible to launch a fully-functional app or website in days using serverless.
* Serverless architecture allows you to build performance-enhancing image and video services for any application. You can use its services to do things like dynamically resize images or change video transcoding for different target devices.

## When to Use Containers? : Containers Use Case

![image info](/img/awscloud/6/What-containers-can-do-better-Copy.png)

*Containers are best to use for Application deployment in following use cases:*

* If you want to use the operating system of your own choice and leverage full control over the installed programming language and runtime version.
* If you want to use software with specific version requirements, containers are great to start with.
* If you are okay in bearing cost of using big yet traditional servers for anything such as Web APIs, machine learning computations, and long-running processes, then you might also want to try out containers as well (They will cost you less than servers anyways)
* If you want to develop new container-native applications
* If you need to refactor a very large and complicated monolithic application, then it’s better to use container as it’s better for complex applications
* Some organizations still use containers to migrate existing applications into more modern environments. Container orchestration tools like Kubernetes comes with already defined best practices which provide ease in managing large-scale container set-up.
* Container orchestration platforms such as Docker can solve your issues with unpredictable traffic (auto-scaling), however, the process of spinning containers up or down won’t be instantaneous.
  
### Conclusion

Which one should you choose? It looks like both have its own benefits. Both can be best or worse solution depending on the use case and there can’t be anything predefined.

If your existing application is big and it exists on-premise, you may first want to run in containers and then slowly move some of its parts towards functions. If you already have a microservice based application and you do not mind vendor lock-in, you can think of going serverless.

Serverless architecture definitely worths your attention especially due to its cost-effectiveness.

As discussed in the introduction, we can’t really say that serverless means the death of containers. Not as of now. However, containers and serverless are both growing technology and it will be interesting to see what each one of them behold.