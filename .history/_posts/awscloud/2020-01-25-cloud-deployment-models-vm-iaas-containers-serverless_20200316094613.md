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

#1. Right Way: Monolithic & Stateful Architecture
Before starting, we assume that you already have a functional app. If not, #2 would be appropriate for you to get started.

The first aspect that we will be evaluating is whether you want to keep the application monolithic and stateful.

Monoliths will require you to compose all of your application within a single piece. It is designed to be served as a self-contained. Different components of the program are interdependent and interconnected to each other.

If any program is to be updated, the whole application has to be rewritten.

However, monoliths have their own benefits. They have better throughput than other architectural patterns. These are also easier to test and debug.

After all these considerations, do you want to keep your architecture monolithic? Are these benefits tops on the list of your application priority? This leads us to our next point.

In case, if you already have an application and want to re-architect the app, may be opting for updated architectural patterns would be the best solution.

#2. When Re-architecting is Not an Option
In this section, we will discuss two types of cases. Firstly, what option do you have available if you are not ready to re-architect the app? Secondly, what if you are getting started from scratch?

To discuss the second case, we assume that the application's first build would be a minimum viable product (MVP). Thus your goal would be to produce an efficient application with faster time to market. In this case, a monolith architecture will be suitable.

Another consideration is feature changes. Sometimes, you'll be at a place where you've bootstrapped an app with well-written specifications and abruptly, there's a change in one of the specs. This is easy with monolithic. 

Here is an interesting image by Martin Fowler which states why going directly to microservices is a dangerous thing:

