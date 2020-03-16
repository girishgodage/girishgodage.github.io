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

#7. Microservices Deployment: Containers & Orchestration
The most adapted cloud deployment model for microservices architecture is containers. Containers help you in having a focused business scope to meet agility in the development and delivery of services.

If you want to use containers on your on-premise data centre or your private cloud, your options include OpenShift Container Platform, Pivotal Container Service or other Kubernetes based platforms.

On the other hand, if you're ready to use the public cloud, your container management options include AWS Fargate, Azure Container Instances, Google App Engine Flex, OpenShift Online, etc. These are simplified container management options at a lower cost but with a loss of control. If you need high control over your container management, your options include Amazon EKS, Google GKS and Azure AKS.

Nevertheless, in the below section, we will have a walk through what technology can be used under what prerequisites and where.


