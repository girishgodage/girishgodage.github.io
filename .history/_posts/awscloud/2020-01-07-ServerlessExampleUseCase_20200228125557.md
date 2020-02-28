---
title: 10 AWS Lambda Use Cases to Start Your Serverless Journey
date: 2020-01-07 10:49:00 Z
permalink: "/blog/serverless-examples-aws-lambda-use-cases"
categories:
- AWSCloud
tags:
- learning
summary: Many big companies such as Netflix, Conde Nast and NY Times are migrating
  their compute services to serverless. But sometimes the cloud architects are confused
  about the application of serverless technologies such as AWS Lambda and Azure functions.
  This blog explains 10 AWS Lambda Use Cases to help you get started with serverless.
  Some of the use cases are Serverless Website | App Authentication | Mass Emailing
  | Real-time Data Transformation | CRON Jobs | Chatbot | IoT.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-serverless-examples
---

### AWS Lambda Use Cases to Start Your Serverless Journey

 Many big companies such as Netflix, Conde Nast and NY Times are migrating their compute services to serverless. But sometimes the cloud architects are confused about the application of serverless technologies such as AWS Lambda and Azure functions. This blog explains 10 AWS Lambda Use Cases to help you get started with serverless. Some of the use cases are Serverless Website | App Authentication | Mass Emailing | Real-time Data Transformation | CRON Jobs | Chatbot | IoT.

 ![image info](/img/awscloud/2/Serverless-Examples-with-AWS-Lambda-Use-Cases.png)

 Netflix, Mapbox, A Cloud Guru, BlackBoard, Conde Nast, and New York Times. Do you know what these companies have in common?

All migrated to serverless architecture and have immensely benefitted from this decision.

At the recently held ServerlessConf, ‘A Cloud Guru’ gave a proof of serverless promise saying they were never required to change their architecture due to performance reasons. They are running 287 Lambda functions, 19 microservices with 3.68 TB of data at the mere cost of $580 per month. Read that again!

**Why we use AWS Lambda?**

AWS Lambda removes the need for the traditional compute services, thus reducing operational costs and complexity. This results in many benefits such as faster development, easier operational management, scaling, and reduction in operational costs.

Moreover if you have frequent changes in memory usage, Lambda takes care of that as well. It has “Pay as you go” model whose billing is based on used memory, number of request and execution duration rounded up to nearest 100 milliseconds. Its huge leap forward in comparison to EC2.

We are well aware of how serverless helps in elevating our focus to more business-critical tasks by empowering developers and offering cost-friendly solutions. However, we have still observed confusion among people who want to get started with serverless adoption but don’t know where, when and how! This blog post is about them.

Let’s start unfolding each of them with practical examples and benefits it brings to the organizations as well as development teams.

* Serverless Website Example with AWS Lambda
* Serverless Authentication Using AWS Cognito
* Multi-Location Media Transformation
* Mass Emailing using AWS Lambda & SES
* AWS Lambda Use Case for Real-time Data Transformation
* Serverless CRON Jobs*
* AWS Lambda Use Case for Efficient Monitoring
* Real-time Notifications with AWS Lambda and SNS
* Building a Serverless Chatbot
* Serverless IoT Backend

**#1. Serverless Website Example with AWS Lambda**

Maintaining a dedicated server is outdated, in fact, even a virtual server. Not only it’s tiresome but provisioning the instances, updating the OS, etc. takes a lot of time and distracts you from focusing on the core functionalities.

**AWS Lambda** along with other AWS services can be used to build a powerful website without having to manage a single server or an operating system. For a basic version, we will use **AWS API Gateway, DynamoDB, Amazon S3 and Amazon Cognito User Pool.**

![image info](/img/awscloud/2/Serverless-Website-Example.png)

The components used here are used for executing the following functionalities:

* JavaScript in the browser exchanges the data from a backend API built through API Gateway and AWS Lambda.
* DynamoDB is a NoSQL database which is used for storing data through API’s Lambda function.
* Amazon S3 is used for hosting the static website content like HTML, media files, CSS, JavaScript which acts as a front end in the user’s browser.
* Amazon Cognito is used for user authentication and management with the help of secured backend API.

The architecture here depicts a basic version of a serverless website. It can be elaborated into a full-fledged multi-functional website by adding other AWS services. Some of the distinguished examples of Serverless websites are:

* Serverless E-commerce Website which uses AWS Lambda use cases for payment management, cart management and recommendation engine.

![image info](/img/awscloud/2/cover-image-1.png)

* **Serverless Dynamic Web Page** built using two Lambda functions to monitor internal services at Monsanto.
* **Serverless Single Page Website** a practical example to develop a low latency website with Lambda functions, S3 and CloudFront.
* **Bustle.com** (news & entertainment website run entirely on serverless) More information can be found here.

**#2. Serverless Authentication Example Using AWS Cognito**
Whether you’re running New York Times or a personal blog, personalization plays a huge role when you interact with your users. **Amazon Cognito** when used with **AWS Lambda**, can empower you to add pre and post-login hooks to execute your custom logic.

After creating an AWS Lambda function, you can trigger it based on various user pool operations such as user sign-up, user confirmation, sign-in, etc. Not only that, you can experiment with your authentication procedure and make it more challenging, migrate the users and send out personalized verification messages, to name a few.

![image info](/img/awscloud/2/Serverless-Authentication-Example-Using-AWS-Cognito.png)

The following are the common triggering sources from where you can hook your Lambda function:

* Sign-up, confirmation and sign-in
* Pre and post authentication
* Custom authentication challenge
* Pre token generation
* Migrate user
* Custom message

Let’s understand how custom message works. Amazon Cognito will trigger your Lambda function before sending an email or phone verification text or multi-factor authentication which allows you to customize the message as per the requirements. The triggering source for the custom message are:

* Confirmation code post-sign-up
* Temporary password for new users
* Resending confirmation code
* Confirmation code to forget password request
* Manual request for new email/phone
* Multi-factor authentication