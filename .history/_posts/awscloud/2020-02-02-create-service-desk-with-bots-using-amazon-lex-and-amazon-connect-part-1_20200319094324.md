---
title: The service desk with bots using Amazon Lex and Amazon Connect (Part 1)
date: 2020-02-02 11:41:00 Z
permalink: "/blog/create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-1"
categories:
- AWSCloud
tags:
- learning
summary: What! Is this guy for real? Does he really think he can replace the front line of IT with pre-canned conversations?" I must admit, it's a bold statement. The IT Service Desk has been around for years and has been the foot in the door for most of us in the IT industry. It's the face of IT operations and plays an important role in ensuring an organisation's staff can perform to the best of their abilities. But what if we could take some of the repetitive tasks the service desk performs and automate them? Not only would we be saving on head count costs, we would be able to ensure correct policies and procedures are followed to uphold security and compliance. The aim of this blog is to provide a working example of the automation of one service desk scenario to show how easy and accessible the technology is, and how it can be applied to various use cases.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: '/blog/create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-1'
nexturl: '/blog/create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-2'
discussion_id: 2020-02-02-create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-1
---

## The service desk with bots using Amazon Lex and Amazon Connect (Part 1)

What! Is this guy for real? Does he really think he can replace the front line of IT with pre-canned conversations?" I must admit, it's a bold statement. The IT Service Desk has been around for years and has been the foot in the door for most of us in the IT industry. It's the face of IT operations and plays an important role in ensuring an organisation's staff can perform to the best of their abilities. But what if we could take some of the repetitive tasks the service desk performs and automate them? Not only would we be saving on head count costs, we would be able to ensure correct policies and procedures are followed to uphold security and compliance. The aim of this blog is to provide a working example of the automation of one service desk scenario to show how easy and accessible the technology is, and how it can be applied to various use cases.

To make it easier to follow along, I've broken this blog up into a number of parts. Part 1 will focus on the high-level architecture for the scenario and begin creating the Lex chatbot.

![image info](/img/awscloud/19/selfservicedora.png)

## Scenario
Arguably, the most common service desk request is the password reset. While this is a pretty simple issue for the service desk to resolve, many service desk staff seem to skip over, or not realise the importance of user verification. Both the simple resolution and the strict verification requirement make this a prime scenario to automate.

## Architecture
So what does the architecture look like? The diagram below dictates the expected process flow. Let's step through each item in the diagram.

![image info](/img/awscloud/19/amazon-connect-page-1-1.png)

### Amazon Connect
The process begins when the user calls the service desk and requests to have their password reset. In our architecture, the service desk uses Amazon Connect which is a cloud based customer contact centre service, allowing you to create contact flows, manage agents, and track performance metrics. We're also able to plug in an Amazon Lex chatbot to handle user requests and offload the call to a human if the chatbot is unable to understand the user's intent.

### Amazon Lex
After the user has stated their request to change their password, we need to begin the user verification process. Their intent is recognised by our Amazon Lex chatbot, which initiates the dialog for the user verification process to ensure they are who they really say they are.

### AWS Lambda
After the user has provided verification information, AWS Lambda, which is a compute on demand service, is used to validate the user's input and verify it against internal records. To do this, Lambda interrogates Active Directory to validate the user.

### Amazon SNS
Once the user has been validated, their password is reset to a random string in Active Directory and the password is messaged to the user's phone using Amazon's Simple Notification Service. This completes the interaction.

## Building our Chatbot
Before we get into the details, it's worth mentioning that the aim of this blog is to convey the technology capability. There's many ways of enhancing the solution or improving validation of user input that I've skipped over, so while this isn't a finished production ready product, it's certainly a good foundation to begin building an automated call centre.

To begin, let's start with building our Chatbot in **Amazon Lex**. In the Amazon Console, navigate to Amazon Lex. You'll notice it's only available in Ireland and US East. As Amazon Connect and my Active Directory environment is also in US East, that's the region I've chosen.

Go ahead and select **Create Bot**, then choose to create your own **Custom Bot**. I've named mine "**UserAdministration**". Choose an Output voice and set the session timeout to 5 minutes. An IAM Role will automatically be created on your behalf to allow your bot to use Amazon Polly for speech. For COPPA, select **No**, then select **Create**.

![image info](/img/awscloud/19/custombot.png)



