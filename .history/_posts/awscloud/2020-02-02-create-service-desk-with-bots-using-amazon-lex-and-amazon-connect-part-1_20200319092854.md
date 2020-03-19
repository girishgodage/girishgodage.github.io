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
prevurl: ''
nexturl: ''
discussion_id: 2020-02-02-create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-1
---

## The service desk with bots using Amazon Lex and Amazon Connect (Part 1)

What! Is this guy for real? Does he really think he can replace the front line of IT with pre-canned conversations?" I must admit, it's a bold statement. The IT Service Desk has been around for years and has been the foot in the door for most of us in the IT industry. It's the face of IT operations and plays an important role in ensuring an organisation's staff can perform to the best of their abilities. But what if we could take some of the repetitive tasks the service desk performs and automate them? Not only would we be saving on head count costs, we would be able to ensure correct policies and procedures are followed to uphold security and compliance. The aim of this blog is to provide a working example of the automation of one service desk scenario to show how easy and accessible the technology is, and how it can be applied to various use cases.

To make it easier to follow along, I've broken this blog up into a number of parts. Part 1 will focus on the high-level architecture for the scenario and begin creating the Lex chatbot.



![image info](/img/awscloud/19/selfservicedora.png)

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

I also have the ability to create prompts. These are audio snippets that can be referenced from a contact flow. I can upload an existing WAV file or I can record one from within the browser:

![image info](/img/awscloud/18/co_record_prompt_1.png)

Next, I can create a new contact flow or edit an existing one. A contact flow defines the customer experience after they make contact. Here is the sample flow that is set up by default:

![image info](/img/awscloud/18/co_sample_flow_2.png)

I can find the desired block in the menu, drag it on to the canvas, and connect it in to the flow:

![image info](/img/awscloud/18/co_create_flow_start_1.png)

Then I click on the header of the block to set it up. I can use a prompt, or I can choose between static or dynamic text to speech (dynamic speech would reference a value attached to the call flow, perhaps resulting from a database lookup):

![image info](/img/awscloud/18/co_setup_prompt_1.png)

One of the most powerful and general blocks allows me to call a Lambda function during the contact flow. The function can pull data from a database, connect to a CRM, or implement any desired type of business logic. The function is provided with key-value pairs as input, and returns the same as output. The returned values are associated with the execution of the flow.

![image info](/img/awscloud/18/co_run_lambda_1.png)

The next step is to create a routing profile, consisting of one or more queues, with associated priorities. The priorities allow agents to service multiple queues in the proper order (lower numbers are higher priority); you can combine priorities and delays to set up routing rules that are in accord with the needs of your users and your contact center:

![image info](/img/awscloud/18/co_add_routing_pro_1.png)

The final step is to add and empower the users (administrators, managers, agents, and so forth). This can be done on a user-by-user basis or by importing a CSV file that contains definitions for multiple users. Here's how I add a user named Hello World:

![image info](/img/awscloud/18/co_add_user_1.png)

I can also create a hierarchy of agents (great for regional and/or divisional reporting).

Amazon Connect lets me use security profiles to control the permissions assigned to each type of user:

![image info](/img/awscloud/18/co_manage_profiles_1.png)

I also have access to operational metrics, both real-time and historical. Here are some of the real-time metrics (the time frame and displayed fields can be customized as needed):

![image info](/img/awscloud/18/co_rt_metrics_1.png)

The metrics are published to CloudWatch, where they can be used to drive alarms.

I hope that this brief tour has given you a reasonably thorough introduction to Amazon Connect, and would love to hear how you have used it to modernize your contact center and to improve the level of service that you can provide to your customers:

![image info](/img/awscloud/18/co_new_call_center_1.png)

As you can see, it is a rich and powerful service, with plenty of room for customization and extension. There are plenty of advanced features and configuration options that you can explore on your own.


Pricing, as I noted, is on a pay-as-you-go basis. You pay for each contact center and each active phone call (inbound or outbound) on a per-minute basis. As part of the AWS Free Tier, you get 90 minutes of contact center usage, two phone numbers (DID and toll-free), 30 minutes of inbound DID calls, 30 minutes of inbound toll-free calls, and 30 minutes of outbound calls, all per month, for an entire year.