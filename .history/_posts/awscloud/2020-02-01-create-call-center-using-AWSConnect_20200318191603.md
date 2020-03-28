---
title: Amazon Connect – Customer Contact Center in the Cloud
date: 2020-02-01 11:41:00 Z
permalink: "/blog/create-call-center-using-AWSConnect"
categories:
- AWSCloud
tags:
- learning
summary: Today I would like to introduce you to Amazon Connect. Building on the same technology used by many of our own customer service teams, Amazon Connect lets you set up a cloud-based contact center in minutes. You create your contact center, design your contact flows (similar to IVRs), and on-board your agents using a modern interface that is entirely web-based.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-02-01-create-call-center-using-AWSConnect
---

## Amazon Connect – Customer Contact Center in the Cloud

Customer service is essential to the success of any business. In order to provide voice-based customer service at scale, many organizations operate call centers. At the low end, call centers simply route incoming calls to any available agent. More sophisticated systems support more sophisticated routing and interaction, including the ability to create customized call trees and other Integrated Voice Response (IVR) systems. Traditionally, IVR systems have been difficult to install and expensive to license, with capacity-based pricing the norm.


![image info](/img/awscloud/18/call_center_4.png)

The Amazon customer service organization aims to deliver an exceptional level of services to our customers. In order to do this at scale, the organization employs tens of thousands of agents working at more than 50 groups and subsidiaries centers scattered around the world, supporting customers that speak a wide variety of languages.

## Welcome to Amazon Connect

Today I would like to introduce you to Amazon Connect. Building on the same technology used by many of our own customer service teams, Amazon Connect lets you set up a cloud-based contact center in minutes. You create your contact center, design your contact flows (similar to IVRs), and on-board your agents using a modern interface that is entirely web-based.

Instead of requiring help from IT teams and specialized consultants, Amazon Connect is simple enough to be configured and run directly by business decision makers. There's no hardware to deploy and no per-agent licenses. Instead, you pay based on the number of customer-minutes and the amount of phone time that you consume. This scalable, pay-as-you-go model means that you can use Amazon Connect in situations where your call volume is unpredictable, spiky, or both.

![image info](/img/awscloud/18/co_switchboard_2.png)

Amazon Connect works with many different AWS services including:

**Amazon S3** – Amazon Connect uses S3 to provide unlimited, encrypted storage of calls (audio) and reports.

**AWS Lambda** – This give you the ability to run code in serverless fashion as part of a customer contact. The code can pull data from your CRM or your database and use the data to provide a personalized customer experience.

**Amazon Lex** – Customers can build natural language contact flows using Amazon Lex to get the same automatic speech recognition (ASR) technology and natural language understanding (NLU) that powers Amazon Alexa. This lets callers simply say what they want instead of having to listen to long lists of menu options and guess which one is most closely related to what they want to do. This feature of Amazon Connect is being released in preview form, and you can request access to the preview if you are interested.

**AWS Directory Service** – Amazon Connect can reference an existing Active Directory or it can create a new one. The directory is used to store user (administrator, manager, or agent) identities and permissions.

**Amazon Kinesis** – Amazon Connect can stream contact trace records (CTRs) to Amazon Kinesis. From there they can be pushed to Amazon S3 or Amazon Redshift and analyzed using Amazon QuickSight or other business analytics tools.

**Amazon CloudWatch** – Amazon Connect publishes real-time operational metrics to CloudWatch. These metrics will tell you how many calls are arriving per second, how many are being held in queues, and so forth. You can use these metrics to observe the performance of your contact center and to make sure that you have the right number of agents on hand.

## Amazon Connect Benefits & Features
Let's take a look at the major benefits and features of Amazon Connect.

* **Cloud-Powered** – Amazon Connect is an integral part of AWS, and is designed to be robust, scalable, and highly available. Each contact center instance runs in multiple AWS Availability Zones.

* **Simple** – As I already noted, Amazon Connect was designed to be set up and run by business decision makers. The graphical console makes the setup process (including the design of graphical call flows) easy and efficient.

* **Flexible** – The Contact Flow Editor (CFE) that powers Amazon Connect includes blocks for interaction, integration, control flow, branching, and more. Call flows can include a mixture of prerecorded audio prompts, generated audio, Lex-powered interaction, integration with existing systems and databases, conversations with agents, call transfers, and more.

* **Economical** – Amazon Connect's pay-as-you-go model keeps operating costs in line with actual usage. On the agent side, Amazon Connect includes a softphone that supports high quality audio.

## Amazon Connect Tour
Let's set up a customer contact center, starting at the Amazon Connect console. I click on Get started:

![image info](/img/awscloud/18/co_console_splash_1.png)

I can choose to use an existing directory or I can create a new one, which I'll do. I'll set up the URL for my contact center to proceed. This URL will allow my contact center's users to sign in:

![image info](/img/awscloud/18/co_step1_choose_dir_2.png)

Next, I set myself up as the administrator:

![image info](/img/awscloud/18/co_step2_create_admin_2.png)

My contact center can accept incoming calls, make outgoing calls, or both. Outgoing calls can be made as part of a contact flow or by an agent. I simply specify what I need (I'll choose my direct dial and/or toll-free numbers later):

![image info](/img/awscloud/18/co_step3_telephony_1.png)

Next, I specify the S3 locations (buckets and prefixes) for call recording and reports. I can use separate buckets and prefixes for each, along with distinct AWS Key Management Service (KMS) encryption keys:

![image info](/img/awscloud/18/co_step4_storage_2.png)

Then I confirm my choices and click on Create instance:

![image info](/img/awscloud/18/co_review_create_2.png)

A few seconds later my Amazon Connect instance is ready to go, and I can click on Get started:

![image info](/img/awscloud/18/co_success_1.png)

Amazon Connect logs me in using the administrator credentials that I entered earlier, and then I click on Let's go to proceed:

![image info](/img/awscloud/18/co_hello_2.png)

Now I can get set up to accept my first call, by choosing a toll free or direct dial phone number:

![image info](/img/awscloud/18/co_claim_num_2.png)

I am ready to accept my first call:

![image info](/img/awscloud/18/co_softphone_3.png)

I call the number on my cell and walk through the options. I press "1" to speak to an agent, and the softphone offers to let me (playing the role of the agent) take the call:

![image info](/img/awscloud/18/co_softphone_pickup_1.png)

Now that the Amazon Connect is set up and accepting calls, I can explore the Dashboard and configure my contact center:

![image info](/img/awscloud/18/co_dash_1.png)

I can see the phone numbers that are associated with the call center:

![image info](/img/awscloud/18/co_my_phones_1.png)

And I can claim additional numbers (up to 10 direct inward dialing and 10 toll free), choosing from numbers in a wide list of countries:

![image info](/img/awscloud/18/co_claim_num_1.png)

At the bottom of the screen I can choose the contact flow that is activated when the number is called:

![image info](/img/awscloud/18/co_pick_flow_1.png)

Returning to the dashboard, the next step is to set the hours of operation. I can see the list of existing hours of operation, and I can create a new one:

![image info](/img/awscloud/18/co_pick_hours_1.png)

I can have multiple hours of operation, each of which can be attached to a queue or referenced from a contact flow.

Now I can create queues to model the routing of contacts (incoming calls) to agents. Queues can represent priorities (different levels of regular and premium support), or agents with different business or language skills.

![image info](/img/awscloud/18/co_make_queue_1.png)
