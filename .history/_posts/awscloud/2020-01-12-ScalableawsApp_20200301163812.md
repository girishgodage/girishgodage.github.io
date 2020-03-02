---
title: How to Build a Scalable Application up to 1 Million Users on AWS
date: 2020-01-12 10:41:00 Z
permalink: "/blog/building-scalable-application-aws-platform"
categories:
- AWSCloud
tags:
- learning
summary: Scalability of an application is equally important as its features and user
  interface. It becomes even more important if your app is going to serve more than
  a million users in the future. In this blog, you will learn how to scale your app
  up to 1 million users on AWS.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-12-scalable-application-aws
---

### Build a Scalable Application

 Scalability of an application is equally important as its features and user interface. It becomes even more important if your app is going to serve more than a million users in the future. In this blog, you will learn how to scale your app up to 1 million users on AWS.

 ![image info](/img/awscloud/4/User-scale-1.png)

 Suppose you’ve built a web application and started getting few customers. After some feedback and suggestions, you are ready with the full-fledged product. Now, your marketing team shares your app on product hunt to acquire new customers. Suddenly, thousands of visitors are using your app and at one point they are unable to use your app.

You’ve tested your app and it is working fine. So what happened?

**“This is not a bug but a problem of scalability. Your cloud architecture is not designed to scale with increasing load.”**

I’ve seen many companies which usually focus more on features and less on scalability. Creating applications that are both resilient and scalable is an essential part of any application architecture. In this blog, you will learn how to build highly scalable web application architecture that can scale with increasing load. 

### What is a Scalable Application?
Scalability refers to the ability of a system to give a reasonable performance under growing demands (This can be larger data-sets, higher request rates, the combination of size and velocity, etc). It should work well with 1 user or 1 million users and handles spikes in traffic automatically. By adding and removing the resources only when needed, scalable apps consume only the resources necessary to meet demand.

When talking about scalability in cloud computing, you will often hear about two main ways of scaling – horizontal or vertical. Let’s look deeper into these terms.

#### Vertical scaling (Scaling Up)

Scaling up or vertical scaling refers to resource maximization of a single unit to expand its ability to handle the increasing load. In hardware terms, this includes adding processing power and memory to the physical machine running the server. In software terms, scaling up may include optimizing algorithms and application code.

![image info](/img/awscloud/4/Single-Host-Vertical-Scalibility.png)

#### Horizontal scaling (Scaling Out)

Scaling out or horizontal scaling refers to resource increment by the addition of units to the app’s cloud architecture. This means adding more units of smaller capacity instead of adding a single unit of larger capacity. The requests for resources are then spread across multiple units thus reducing the excess load on a single machine.

![image info](/img/awscloud/4/Single-Host-Horizontal-Scalibility.png)

### Characteristics of the Scalable Application
What does scalability look like? There are some areas where an app needs to excel to be considered scalable.

#### Performance
First and foremost, the application must operate well under stress with low latency. The speed of a website affects usage and user satisfaction, as well as search engine rankings, a factor that directly correlates to revenue and retention. As a result, creating a scalable web application architecture that is optimized for fast responses and low latency is key.

#### Availability and Reliability
These are closely related and equally necessary. Scalable apps rarely if ever go down under stress. They need to reliably produce data upon request and not lose stored information.

#### Manageability
The manageability of the cloud architecture equates to the scalability of operations: maintenance and updates. Things to consider for manageability are the ease of diagnosing and understanding problems when they occur, ease of making updates or modifications, and how simple the system is to operate. (i.e., does it routinely operate without failure or exceptions?)

#### Cost
Highly scalable applications don’t have to be unreasonably expensive to build, maintain, or scale. Planning for scalability during development allows the app to expand as demand increases without causing undue expenses.

You’ve plenty of options to choose the cloud provider while building the high-performance web application architecture. The three leading cloud computing vendors, AWS, Microsoft Azure and Google Cloud, each have their own strengths and weaknesses that make them ideal for different use cases.

In this blog, I’ve chosen AWS to show you how to build web scalable application. AWS is a subsidiary of the renowned company, Amazon, it provides different services that are cloud-centered for various requirements. AWS holds the highest 33% market share of cloud computing. They provide excellent documentation on each of their services, helpful guides and white papers, reference architectures for common apps.

### Steps to build a scalable application based on increasing users from 1 to 1 million

* Step1 : Initial Setup of Cloud Architecture
* Step2 : Create a Multiple Hosts and Choose Database
* Step3 : Store database on Amazon RDS
* Step4 : Create a multiple avialibility zones
* Step5 : Move Static content to Object-based Storage
* Step6 : Auto Scaling   
* Step7 : Service Oriented Architecture(SOA)

> ### 1 User
  ### Initial Setup of Cloud Architecture
  * Amazon Machine Images (AMI)
  * Amazon EC2
  * Amazon VPC
  * Amazon Route 53 

**Amazon Machine Images (AMI)**

Amazon Machine Image (AMI) gives the information required to launch an instance, which is a virtual server in the cloud. You can specify an AMI during the launch an instance. An AMI includes a template for the root volume for the instance, launch permissions that control which AWS accounts can use the AMI to launch instances and a block device mapping that specifies the volumes to attach to the instance when it’s launched.

**Amazon Elastic Compute Cloud (Amazon EC2)**

Amazon Elastic Compute Cloud provides the scalable computing capacity in the AWS cloud. This eliminates the hardware upfront so that you can develop and deploy applications faster.

**Amazon Virtual Private Cloud (Amazon VPC)**

Amazon Virtual Private Cloud gives a provision to launch AWS resources in a virtual network. It gives complete control over the virtual networking environment including a selection of IP address range, subnet creation, the configuration of route tables and network gateways.

**Amazon Route 53**

Amazon Route 53 is a highly available and scalable cloud DNS web service. Amazon Route 53 effectively connects user requests to infrastructure running in AWS – such as Amazon EC2 instances, Elastic Load Balancing load balancers or Amazon S3 buckets.

Here you need a bigger box. You can simply choose the larger instance type which is called vertical scaling. At the initial stage, vertical scaling is enough but we can’t scale vertically indefinitely. Eventually, you’ll hit the wall. Also, it doesn’t address failover and redundancy.

### USERS > 10
### Create multiple hosts and choose the database
First, you need to choose the database as users are increasing and generating data. It’s advisable to start with SQL Database first because of the following reasons:

* Established and well-worn technology.
* Community support and latest tools.
* We aren’t going to break SQL DBs in our first 10 million users.

Note, you can choose the NoSQL database if your users are going to generate a large volume of data in various forms. 

At this stage, you have everything in a single bucket. This architecture is harder to scale and complex to manage in the long run. It’s time to introduce the multi-tier architecture to separate the database from the application.

### USERS > 100
### Store database on Amazon RDS to ease the operations
When users increase to 100, Database deployment is the first thing which needs to be done. There are two general directions to deploy a database on AWS. The foremost option is to use a managed database service such as Amazon Relational Database Service (Amazon RDS) or Amazon Dynamo DB and the second step is to host your own database software on Amazon EC2.

* Amazon RDS
* Amazon DynamoDB

**Amazon RDS**

Amazon Relational Database Service (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. Amazon RDS provides six familiar database engines to choose from, including Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL and MariaDB.

### User > 1000
### Create multiple availability zones to improve availability
As per current architecture, you may face availability issues. If the host for your web app fails then it may go down. So you need another web instance in another Availability Zone where you will put the slave database to RDS.

* Elastic Load Balancer (ELB)
* Multi – AZ Deployment

Here you’ve to use Elastic Load Balancer (ELB) to balance the load between the two web host instances in the two AZs.

**Elastic Load Balancer (ELB)**

ELB distributes the incoming application traffic across EC2 instances. It is horizontally scaled, imposes no bandwidth limit, supports SSL termination, and performs health checks so that only healthy instances receive traffic.

This configuration has 2 instances behind the ELB. We can have 1000s of instances behind the ELB. This is Horizontal Scaling.

At this stage, you’ve multiple EC2 instances to serve thousands of users which ultimately increases your cloud cost. To reduce the cost, you have to optimize instances’ usage based on varying load. 

### Users: 10,000s – 100,000
### Move static content to object-based storage for better performance
To improve performance and efficiency, you’ll need to add more read replicas to RDS. This will take load off the write master database. Furthermore, you can reduce the load from web servers by moving static content to Amazon S3 and Amazon CloudFront.

* Amazon S3
* Amazon CloudFront
* Amazon DynamoDB
* Amazon ElastiCache

**Amazon S3**

Amazon S3 is object-based storage. It is not attached to EC2 instance which makes it best suitable to store static content, like javascript, CSS, images, videos. It is designed for 99.999999999% of durability and can store multiple petabytes of data.

**Amazon CloudFront**

Amazon CloudFront is a Content Delivery Network(CDN). It retrieves data from Amazon S3 bucket and distributes it to multiple data center locations. It caches content at the edge locations to provide our users with the lowest latency access.

Furthermore, to reduce the load from database servers, you can use DynamoDB(managed NoSQL database) to store session state. For caching data from the database, you can use Amazon ElastiCache.

**Amazon DynamoDB**

Amazon DynamoDB is a fast and flexible NoSQL database service for applications that need consistent, single-digit millisecond latency. It is a completely managed cloud database and supports document and key-value store models.

**Amazon ElastiCache**

Amazon ElastiCache is a Caching-as-a-Service. It removes the complexity associated with deploying and managing a distributed cache environment. It’s a self-healing infrastructure if nodes fail new nodes are started automatically.

### Users > 500,000
### Setting up Auto Scaling to meet the varying demand automatically
At this stage, your architecture is quite complex to be managed by the small team and without proper monitoring, analyzing it’s difficult to move forward.

* Amazon CloudWatch
* AWS Elastic Beanstalk
* AWS OpsWorks
* AWS Cloud Formation
* AWS 

Now that the web tier is much more lightweight, it’s time for Auto Scaling!

> “Auto Scaling is nothing but an automatic resizing of compute clusters based on demand.”

Auto Scaling enables “just-in-time provisioning,” allowing users to scale infrastructure dynamically as load demands. It can launch or terminate EC2 instances automatically based on Spikes in Traffic. You pay only for the resources which are enough to handle the load.

![image info](/img/awscloud/4/Amazon-1.png)

For monitoring you can use the following AWS services:

**Amazon CloudWatch**

AWS CloudWatch provides a rich set of tools to monitor the health and resource utilization of various AWS services. The metrics collected by CloudWatch can be used to set up alarms, send notifications, and trigger actions upon alarms firing. Amazon EC2 sends metrics to CloudWatch that describe your Auto Scaling instances.

The autoscaling group can include multiple AZs, up to as many as are in the same region. Instances can pop up in multiple AZs not just for scalability, but for availability.

We need to add monitoring, metrics, and logging to optimize Auto Scaling efficiently.

* **Host-level metrics**. Look at a single CPU instance within an autoscaling group and figure out what’s going wrong.
* **Aggregate level metrics**. Look at metrics on the Elastic Load Balancer to understand the performance of the entire set of instances.
* **Log analysis**. Look at what the application is telling you using CloudWatch Logs. CloudTrail helps you analyze and manage logs. If you have set up region-specific configurations in CloudWatch, it is not easy to combine metrics from different regions within an AWS monitoring tool. In that case you can use Loggly, a log management tool. You can send logs and metrics from CloudWatch and CloudTrail to Loggly and unify these logs with other data for a better understanding of your infrastructure and applications.

Squeeze as much performance as you can from your configuration. Auto Scaling can help with that. You don’t want systems that are at 20% CPU utilization.

The infrastructure is getting big, it can scale to 1000s of instances. We have read replicas, we have horizontal scaling, but we need some automation to help manage it all, we don’t want to manage each individual instance. Here some automation tools:

**AWS Elastic Beanstalk**

AWS Elastic Beanstalk is a service that allows users to deploy code written in Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, NGINX, Passenger, and IIS.

**AWS OpsWorks**

AWS OpsWorks provides a unique approach to application management. Additionally, AWS OpsWorks auto-heals application stack, giving scaling based on time or workload demand and generates metrics to facilitate monitoring.

**AWS Cloud Formation**

AWS Cloud Formation provides resources using a template in JSON format. You have the option to choose from a collection of sample templates to get started on common tasks.

**AWS CodeDeploy**

AWS Code Deploy is a platform service for automating code deployment to Amazon EC2 instances and instances running on premises.

### Users > 1 million
### Use Service Oriented Architecture(SOA) for better flexibility
To serve more than 1 million users you need to use Service Oriented Architecture(SOA) while designing large scale web applications.

 
* Amazon Simple Queue Service (SQS)
* Amazon Simple Notification Service (SNS)
* AWS Lambda

In SOA, we need to separate each component from the respective tiers and create separate services. The individual services can then be scaled independently. Web and application tiers will have different resource requirements and different services. This gives you a lot of flexibility for scaling and high availability.

AWS provides a host of generic services to help you build SOA infrastructure quickly. They are:

**Amazon Simple Queue Service (SQS)**

It is a simple and cost-effective service to decouple and coordinate the components of a cloud application. Using SQS sending, storing, and receiving messages can be executed easily between software components of any size.

**Amazon Simple Notification Service (SNS)**

With SNS you can send messages to a large number of subscribers. The benefits are easy setup, smooth operation, and high reliability to send notifications to all endpoints.

**AWS Lambda**

AWS Lambda is a compute service that lets you run code without provisioning or managing servers. AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. You pay only for the compute time you consume – there is no charge when your code is not running. You can also build serverless architecture composed of functions that are triggered by events.

### Conclusion
The decision about how to approach scaling should be made up front because you never know when you are going to get popular! Also, crashing (or even just slow) pages leave your users unhappy and your app with a bad reputation. It ultimately affects your revenue.

Learning how to build scalable websites takes time, a lot of practice, and a good amount of money. For companies that have a need and struggling to get this done, We are your best option for such a project. We’ve created Scalable Web Apps for clients having millions of users. Contact us for a consultation and price quote.
