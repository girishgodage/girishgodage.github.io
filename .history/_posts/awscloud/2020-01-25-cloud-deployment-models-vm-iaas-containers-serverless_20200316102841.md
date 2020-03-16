---
title: How to Choose the Right Cloud Deployment Model for your Application?
date: 2020-01-25 11:41:00 Z
permalink: "/blog/cloud-deployment-models-vm-iaas-containers-serverless"
categories:
- AWSCloud
tags:
- learning
summary: Understand and analyze different Cloud Deployment Models- public, private, hybrid and community. Select the perfect Cloud Deployment Models based on your App Architecture,VMs vs IaaS vs Containers vs Serverless. Read more to find out their performance & potential use cases.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-25-cloud-deployment-models-vm-iaas-containers-serverless
---

## How to Choose the Right Cloud Deployment Model for your Application?

Understand and analyze different Cloud Deployment Models: public, private, hybrid and community. Select the perfect Cloud Deployment Models based on your App Architecture: VMs vs IaaS vs Containers vs Serverless. Read more to find out their performance & potential use cases.

![image info](/img/awscloud/17/cover-image4.png)

When companies talk about moving to the cloud, it is a general assumption that they are bringing their on-premise workload to the public cloud and not switching clouds. However, GitLab, a startup famous for providing developers tools, announced earlier this year that they are abandoning Microsoft Azure and moving to Google Cloud Platform, as mentioned in the official GitLab statement.

This step took place because GitLab wanted to adopt cloud-native practices with the best use of microservices and containers. The same approach used by GitLab is becoming a critical factor for modern software development. Also, Kubernetes turned out their preferable choice since it allows elastic scaling from a couple of users to millions.

GitLab is just not one example! There are many organizations who switch their cloud deployment models amid the requirements demanded by the modern application users.

You could be also one of the people who is reconsidering their public cloud choices. Maybe because your needs have changed? Or you could be one wondering to re-architect an application? The possibilities are endless.

If you are one of those people, this blog is about you. There are many considerations which need to be overlooked when you're deciding the architecture, technology and cloud deployment models for your application.

Before we move further, let's discuss the four basic cloud deployment models:

## What are Cloud Deployment Models?

Cloud Deployment models are a particular type of cloud environments. They are basically classified based on access, proprietorship and size of the environment. There are four types of cloud deployment models: 

* Public Cloud Deployment Model
* Private Cloud Deployment Model
* Hybrid Cloud Deployment Model
* Community Cloud Deployment Model 

There are many cloud deployment models, each one with their own specifications, benefits and trade-offs for the people who are reconsidering their options. Since each one should be used for the different set of requirements, it is highly critical for you to have a clear understanding of your options. Let's discuss all of them.

### #1. Public Cloud Deployment Model
This is the type of cloud deployment model in which the infrastructure is open for anybody to use. This has a distinct benefit of being more secure than transferring information via the Internet. This also costs less than the private cloud deployment model since the services are more commoditized. This is used by businesses which require on-demand scaling, for example, social networking, CRM, storage, etc.

###  #2. Private Cloud Deployment Model
This is a type of cloud deployment model in which the infrastructure is provisioned exclusively for a single company which may or may not include consumption through multiple business units.

It can be either on or off premises. This type of cloud deployment model is incorporated when sensitive or mission-critical information is involved. This allows for higher security, reliability and performance. This is mostly incorporated by governments and scientific laboratories.

###  #3. Hybrid Cloud Deployment Model
This consists of two or more than two cloud deployment models. All these models are unique but are bound by certain standard protocols. There are very few companies which have the ability to switch over all of their technology stacks to the cloud in one go.

For such companies, a hybrid cloud deployment model provides a smooth transition with the mix of on-premise and cloud options. NASA uses a hybrid cloud deployment model. ***For example, Nebula, an open-source cloud computing project employs a private cloud for research and development purposes while uses the public cloud to share datasets with the external public and partners.***

### #4. Community Cloud Deployment Model
This is a type of a cloud deployment model in which the cloud hosting is shared between many companies provided that they are operating within the same domain, for example, banking, government, educational institution, etc.

In other words, a group of several companies share a multi-tenant setup where they have the same privacy, security and performance concerns. This is used by businesses in joint ventures and research firms that require centralized cloud computing systems.

This was to give you a basic idea of the cloud deployment options that are available to you. But selecting the right one for your business is a tricky subject. Foremost of them, it highly depends on your application architecture. Especially when there are multiple viable and noble options depending upon whether you're a well-established organization with million users or you're a startup who are trying to build their MVP.

In this blog, we are going to discuss that! Which architecture you should opt for? What are the possible cloud deployment models available? What are the pros and cons? Let's figure out.

![image info](/img/awscloud/17/4-1.png)

Considering the above flowchart, I assume you have got a better idea about which cloud deployment model you want to use. However, we will discuss each of the points in detail in the following section.

## #1. Right Way: Monolithic & Stateful Architecture
Before starting, we assume that you already have a functional app. If not, #2 would be appropriate for you to get started.

The first aspect that we will be evaluating is whether you want to keep the application monolithic and stateful.

Monoliths will require you to compose all of your application within a single piece. It is designed to be served as a self-contained. Different components of the program are interdependent and interconnected to each other.

If any program is to be updated, the whole application has to be rewritten.

However, monoliths have their own benefits. They have better throughput than other architectural patterns. These are also easier to test and debug.

After all these considerations, do you want to keep your architecture monolithic? Are these benefits tops on the list of your application priority? This leads us to our next point.

In case, if you already have an application and want to re-architect the app, may be opting for updated architectural patterns would be the best solution.

## #2. When Re-architecting is Not an Option
In this section, we will discuss two types of cases. Firstly, what option do you have available if you are not ready to re-architect the app? Secondly, what if you are getting started from scratch?

To discuss the second case, we assume that the application's first build would be a minimum viable product (MVP). Thus your goal would be to produce an efficient application with faster time to market. In this case, a monolith architecture will be suitable.

Another consideration is feature changes. Sometimes, you'll be at a place where you've bootstrapped an app with well-written specifications and abruptly, there's a change in one of the specs. This is easy with monolithic. 

Here is an interesting image by Martin Fowler which states why going directly to microservices is a dangerous thing:

![image info](/img/awscloud/17/path.png)

Coming to the case where you don't want to re-architect the app, opting for a service-oriented architecture would be a better solution.

***Mark Richards in his book Microservices vs Service Oriented Architecture suggests that SOA is better suited for comparatively large, enterprise-wide systems that require integration with many heterogeneous applications and services.***

## #3. Public/Private Cloud Options: VMs vs IaaS

Public clouds are the most common way of deploying your applications since everything is owned and operated by third-party service providers.

Or maybe if you want to increase your current capacity without much investment, IaaS provides you with the on-demand services. Saying this, it is ideal for a small startup of a mid-sized business.

Another reason being there are no upfront costs to get yourself started and you pay for what your use. Resources are available on-demand to meet your business needs. Some of the IaaS providers are **AWS, DigitalOcean, Microsoft Azure, Rackspace Open Cloud, Alibaba Cloud, Google Compute Engine, etc.**

If you do not want to opt for public cloud offerings, VMs could be a probable solution for you. They are suitable if latency is a key factor in your application.

Some of the prominent VMs providers are RedHat, Oracle, VMWare and many more.

## #4. Analyzing Requirements of Event-driven Architecture
An event-driven architecture is a development technique where your architecture orchestration will revolve around the production, detection and consumption of events along with the responses they evoke. It works on the principle of event publisher/subscriber and with an event manager which is a middleman between both.

A few questions to ask before you consider opting for event-driven architecture:

* ***Is your current app a small and usable building block? Can it be broken down into small blocks any further?***
  
* ***The outputs and inputs of all the blocks are well-defined?***
* ***Would you be able to use current development and build tools?***

* ***Does the serverless platform you're opting for supports the following:***
  
**#1.** Receiving and sending connections from the current APIs/clients?

**#2.** Interacting with the current database?

**#3.** Languages you've hands-on experience with?

If you think event-driven architecture is your potential option, you may go for **microservices or serverless deployment options** about which we will discuss in the next section. If no, modular/microservices are better-suited options for you? Let's find out.

## #5. FaaS Deployment Options: Public vs Private
Now that you've decided to opt for event-driven architecture. There are many serverless providers which offer functions-as-a-service on pay-per-use billing model. Or you can also opt for open source options to use at your on-premise data-centres.

FaaS has its own benefits like easy integration, no resource management, extremely inexpensive (based on your usage though) and on-demand scalability. Some of the major FaaS providers are **AWS Lambda, Azure Functions, Cloud Functions, IBM Cloud, Nano Lambda, Syncano, Twilio and many more.**

However, if you deploy functions on your on-premise data centres, you have the freedom of unlimited runtime and no cold start issues. ***Here are some of your options: Apache OpenWhisk, Oracle Fn Project, Kubeless, OpenFaas, Nuclio and many more. For more information on open source serverless options, visit here: Today in Serverless & Open Source***

Before you opt for either of the cloud deployment model: public or private cloud, I'd suggest having a basic understanding of how vendor lock-in can affect your choices.

***For example, if you've got a common standard then you have multiple vendors to choose from. If you're self-hosting and can run on any x64 box then it doesn't matter if HP (say) decides they want to increase the price of their servers, you just buy them from somewhere else***.

If you've got a rack of servers in a managed datacenter you can put them in a truck and move them to another rack at a competitor (the rack fittings are standard). If you're running basic VM images and AWS prices rise then you can take those same images to Azure.

But there's not yet anything like that for lambda: code that uses lambda only runs on AWS because there's no common standard for the serverless/lambda interface, each cloud provider has their own way of doing it.

## #6. Modular Services & Microservices
Microservice architecture is a distinct method of developing the software/apps wherein organizations who want to work into an agile environment and move towards DevOps and continuous testing.

This allows you to create scalable solutions. It enables you to deliver new features into their live production systems multiple times a day, as well as to support web-scale traffic volumes. This is one of the major reasons why tech giants like **Netflix, eBay, Amazon, Twitter, PayPal and many others work upon this.**

Hence, here are the 5 point roadmap you need to answer to analyze whether you're microservice ready or not?

  1. Determine whether your team is mature enough to adopt microservices. Whether or not you have a strong and disciplined architectural practice.
  2. Do you have a mature agile and DevOps practice? Is your data management team supporting it?
  3. If yes, take the process slowly. Initially, use the microservices only for the part where you need it, for examples, in applications with scalability and web-scale agility requirements.
  4. What will be your deployment environments? About which we will talk further. However, you will need to supplement it with capabilities such as API gateways, service discovery, monitoring and telemetry, build and test automation, messaging and data persistence.
  5. If your team isn't ready to adopt the microservices architecture in its entirely, consider using modular services to gain relevant experience and significantly agility benefits.
   
Modular services, on the other hand, are loosely coupled arrangement of objects. This has no specific architecture like SOA, however, do not have the granularity as much as microservices. All the objects are defined according to business capabilities. All these objects interact with each other through the well-defined common interface.

If this sounds good for you, let's analyze this aspect with little more considerations in the next section.

## #7. Microservices Deployment: Containers & Orchestration
The most adapted cloud deployment model for microservices architecture is containers. Containers help you in having a focused business scope to meet agility in the development and delivery of services.

If you want to use containers on your on-premise data centre or your private cloud, your options include OpenShift Container Platform, Pivotal Container Service or other Kubernetes based platforms.

On the other hand, if you're ready to use the public cloud, your container management options include AWS Fargate, Azure Container Instances, Google App Engine Flex, OpenShift Online, etc. These are simplified container management options at a lower cost but with a loss of control. If you need high control over your container management, your options include Amazon EKS, Google GKS and Azure AKS.

Nevertheless, in the below section, we will have a walk through what technology can be used under what prerequisites and where.

When to use VMs?
VMs are the heart of cloud computing. Significant adoption of virtualization has led to major changes in the processor architecture. Let's discuss some of the scenarios where you should be using VMs:

High Security: With no added alterations, virtual machines provide greater security than other cloud deployment models. The major advantage here is that it has isolated hardware. On the other side, containers have shared kernel resources and application libraries. That says if security is an adamant prerequisite for you, VMs would be the best solution. 
Disaster Recovery: Major benefit of VM is the low turnaround time for site recovery and high availability. Although physical infrastructure also offers these capabilities, however, virtualization offers a wide range of options.
Portable Environments: VMs are basically self-contained packages which are transferable. Virtualization offers you the ability to easily move from one server to another. This is highly critical when you want to balance the workload.
Testing Environment: VMs offers multiple test environments. With VMs you can try different operating systems or may just different versions of a single operating system rather than having different computers for each one of them.
When to use IaaS?
IaaS in other sense is a virtual data centre. It can be used for a variety of purposes. You can install and make use of the operating system you like.

Temporary Workload: Unlike VMs, servers, storage, network resources and data centre is managed by the service providers. This makes IaaS a perfect option to opt for in cases where you want to deal with the temporary workload (say, during Christmas or Black Friday Sale) without much of a hassle. This lets you extend your data centre capabilities. 
Data analysis: To perform a huge analysis of the data, large computing power is required. IaaS provides you with the option to buy that computing power on demand in the most economical way. There are plenty of organizations using IaaS for data analysis and mining purposes be it for structured data or unstructured.
Networking Services: IaaS provides you to expand your networking services whenever the complexity grows. This could be for a short-term big data project or to support the ongoing services. This can free up internal IT staff for other priorities.
High Scalability: Instead of investing upfront in purchasing the hardware, IaaS provides you with the virtualized computing resources. This lets you scale your workloads as you move forward on the pay-for-use model.
When to use Containers?
If you are a mid-sized company and wants to run an application without the need for virtual machines, containers are for you. Operating through multiple operating systems can be a tedious task, containers let you run everything on a single host with access to the single kernel.

Single OS: If you want to make use of an operating system of your own choice and want full control over your runtime language and version, containers work for you. Containers are great to start with when you want to run your software with specific version requirements.
High Agility: Containers are comparatively faster than virtual machines because they have less overhead. This makes the development teams move fast, deploy software efficiently and operate at an unprecedented scale.
Isolated & consistent environment: Containers provide a predictable environment to the development teams which are isolated from the other applications. These can also include software libraries, programming language runtimes and software dependencies with itself.
High Portability: Containers can run anywhere you want which includes operating systems like Linux, Windows and Mac. On VMs or bare metal, in data centres on-premise or cloud and, of course, on the developer's machine. This provides a highly portable environment for you to run your software.
It's kind of embarrassing when you're the premiere cloud infrastructure vendor in the world and you can't keep up with demand on your own major shopping day, when scaling on demand is, you know one of the primary benefits of IaaS.

— Ron Miller (@ron_miller) July 18, 2018

Let's understand more from the example of Github moving to Container Orchestration Framework: As the number of services they ran increased, the SRE team began supporting similar configurations for dozens of other applications, increasing the percentage of our time we spent on server maintenance, provisioning, and other work not directly related to improving the overall GitHub experience.

New services took days, weeks, or months to deploy depending on their complexity and the SRE team's availability. Over time, it became clear that this approach did not provide our engineers with the flexibility they needed to continue building a world-class service.

Their engineers needed a cloud deployment model which acts as a self-service platform they could use to experiment, deploy, and scale new services. Thee also needed that same platform to fit the needs of their core Ruby on Rails application so that engineers and/or robots could respond to changes in demand by allocating additional compute resources in seconds instead of hours, days, or longer.

Read more about Containers vs Serverless: Comparing the Application Deployment Options.

When to use Serverless?
If you're a company already using microservices and want to take advantage of the event-driven architecture, serverless is your option. This is especially true when you're dealing with unpredictable traffic, releasing quicker updates and anything in between.

Variable and irregular workloads: For example, a website which doesn't get much or any traffic during the nights. Since you're required to pay only for the time your code runs, this is a potential cost optimizer. 
Critical developer productivity: If you do not have the example in either Serverless or Containers, serverless would be a far better option for you to ship your Hello World application. However, if you want to run a complex application, a container-based application is a better option. When you use containers, you will have to create a cluster, configure your orchestration framework and then deploy the first container. On the other hand, serverless platforms use web CLI provided by the cloud provider to get started in minutes.
In-built auto-scalability: The greatest benefit of serverless is its auto-scalability. If you're a developer you probably don't want to manage anything. FaaS providers manage everything for you and provide seamless auto-scalability.
Frequent updates: Serverless doesn't require you to upload your code to the servers or do any backend configuration in order to release a working version of an application. This lets you release quicker releases on your product. You can either upload code all at once or a function at a time since your application isn't a monolithic stack but rather a collection of functions provisioned by the vendor.
Take Away!
While you evaluate your cloud deployment model, it is critical to consider your application architecture as well. If you haven't already upgraded your application architecture and respective cloud deployment model, it's likely just a matter of time before you do.

Choosing the best possible deployment options for your business is vital to your company's success, meaning you need to fully understand all the advantages and disadvantages of various options to make the right choice.
