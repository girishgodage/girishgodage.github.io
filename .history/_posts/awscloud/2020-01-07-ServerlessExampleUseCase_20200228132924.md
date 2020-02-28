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

**#3. AWS Lambda Use Case for Multi-Location Media Transformation**

With the rising number of global viewership, we all know how difficult a task it is to facilitate media files in multiple formats on multiple locations. With the prerequisite of least processing time, reducing the latency and minimizing the bandwidth is important than ever. Here are some common scenarios which you might have come across:

* Resizing image based on the query parameter
* Serving appropriate file format based on browser characteristics, for example, WebP for Chrome/Android browsers and JPEG for the rest
* Defining whitelist of dimensions to be generated

This problem can be simplified with the help of Lambda@Edge and CloudFront. This process can be executed by adding four Lambda triggers to CloudFront. Here’s how it works:

![image info](/img/awscloud/2/AWS-Lambda-Edge-Multi-Location-Media-Transform-Example-768x239.png)

* **Lambda 1 (viewer request):** This function is executed to serve the media file in the requested format from the CloudFront cache. No further functions will be executed.
* **Lambda 2 (origin request):** If the requested format is not available, this function fetches the media file with requested configurations from the Amazon S3 bucket and cache it to CloudFront. If the file doesn’t exist, Lambda 3 executed.
* **Lambda 3 (origin response):** This function makes a network call which fetches the original image from the S2 bucket, transforms it as per the requirement and uploads it back.
* **Lambda 4 (viewer response):** This function serves the requested media file from the CloudFront cache.

Note: Lambda 2,3 and 4 are executed only when the requested media file isn’t available in the cache. Here’s more you can do with Lambda@Edge:

* Inspect cookies and rewrite URLs to perform A/B testing.
* Send specific objects to your users based on the User-Agent header.
* Implement access control by looking for specific headers before passing requests to the origin.
* Add, drop, or modify headers to direct users to different cached objects.
* Generate new HTTP responses.
* Cleanly support legacy URLs.
* Modify or condense headers or URLs to improve cache utilization.
* Make HTTP requests to other Internet resources and use the results to customize responses.

**#4. Mass Emailing using AWS Lambda & SES**
New York Times sends our 4 Billion emails per year which includes newsletters, breaking news and transactional emails. Mass mailing is an integrated part of marketing services for any organizations. Traditional solutions often require hardware expenditure, license costs and technical expertise.

With **AWS Lambda** and **Simple Email Service SES**, you can build a cost-effective and in-house serverless email platform. Along with S3 (where your mailing list will be stored) you can quickly send HTML or text-based emails to a large number of recipients.

![image info](/img/awscloud/2/Serverless-Email-Example-using-SES-768x205.png)

Whenever a user uploads a CSV file, it triggers an S3 event. This event triggers another Lambda function which imports the file into the database and will start sending email to all the addresses. For sending our scheduled newsletters, you can integrate it with CloudWatch Events.

Here’s an example of MoonMail’s (email marketing platform) serverless technology stack built using AWS Lambda use cases and SES. The primary reason for them to switch to serverless is extremely fast performance and infinite scalability.

**#5. AWS Lambda Use Case for Real-time Data Transformation**

**Amazon Kinesis Firehose** is basically used for writing real-time streaming data to **Amazon S3, Redshift or Elasticsearch**. But business requirements have changed over the time.

Sometimes it is required to amend or restructure the raw data for before writing it to the destination. Some of the common use cases that have emerged over the time are:

* Normalize the data acquired from different touchpoints
* Adding metadata to the recorded data
* Converting or restructuring the data as per the destination prerequisites
* Performing ETL functionality
* Combine data from another data source

Fulfilling such requirements is now possible with **AWS Lambda** use cases. It helps you create a powerful and scalable way to execute data transformations on the clickstream data.

After buffering the incoming data from the source destinations, Firehose invokes a Lambda function asynchronously over a specified batch period. This Lambda function transforms the data as per the custom logic and sends it back to Firehose. From here, the data is written to the specified destination.

![image info](/img/awscloud/2/Real-time-Data-Transform-AWS-Lambda-Kinesis-Firehose.png)

Along with this, you also have an option to store the raw data (source data backup) to S3 and create a raw data lake before transforming. This process happens concurrently along with your data transformation.

To help you get started with this functionality, AWS provides you with predefined Lambda blueprints in the following format.

* Syslog to JSON
* Syslog to CSV
* Apache Log to JSON
* Apache Log to CSV
* General Firehose Processing

With 5 more use cases left in the Serverless journey, I expect some of you to be slightly anxious of the cost of running a serverless application.

![image info](/img/awscloud/2/AWS-Lambda-pricing-768x368.png)

**#6. Serverless CRON Jobs Example**

CRON or scheduled jobs which are executed based on different parameters are used widely for executing simple time-based tasks. CloudWatch Events now supports cron-like expressions which can be used to trigger a Lambda function periodically.

![image info](/img/awscloud/2/Serverless-CRON-Jobs-Example.png)

Simply create a Lambda function and direct AWS Lambda to execute it on a regular schedule by specifying a fixed rate or cron expression. While creating your Lambda function,  you need to provide CloudWatch Events as an event source and specify a time interval. For example, create a new event every y and invoke this Lambda function with it. Some real-life use case could be:

* If you are running a membership site where accounts have an expiration date, you can schedule a cron job to regularly deactivate the expired account
* Sending out the newsletter on fixed timings
* Cleaning up the database cache on the regular interval

Here’s an example to help you get started with CRON jobs in CloudWatch and AWS Lambda with Node.Js

**#7. AWS Lambda Use Case for Efficient Monitoring**
**CloudWatch Events** delivers a real-time stream of system events that depicts changes in AWS resources.  Before the CloudWatch was launched as an event source, you needed to use the CloudWatch Event console or APIs to integrate it with Lambda.

By creating CloudWatch Event rules, you can monitor and create Lambda functions for processing. Two general scenarios where you can use this possibility:

![image info](/img/awscloud/2/AWS-Lambda-Triggers-using-CloudWatch-Metrics-768x499.png)