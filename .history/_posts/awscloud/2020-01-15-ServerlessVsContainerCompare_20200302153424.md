---
title: Serverless vs Containers A Face-off between your Application Deployment Options
date: 2020-01-15 10:41:00 Z
permalink: "/blog/serverless-vs-containers"
categories:
- AWSCloud
tags:
- learning
summary:  Like most of the existing cloud application development trends, Serverless and Containers have been widely spoken-of and discussed among developers. Some feel that Serverless originated as an alternative to containerization and some feel serverless can be used with containers for application deployment. The difference between Serverless and Containerization is not limited to performance, but also applicable for longevity limitations, application scalability, time-of-deployment etc.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-15-serverless-vs-containers
---

## serverless-vs-containers

  Like most of the existing cloud application development trends, Serverless and Containers have been widely spoken-of and discussed among developers. Some feel that Serverless originated as an alternative to containerization and some feel serverless can be used with containers for application deployment. The difference between Serverless and Containerization is not limited to performance, but also applicable for longevity limitations, application scalability, time-of-deployment etc.

 ![image info](/img/awscloud/6/container-function-1 (1).png)

 When it comes to serverless providers, there are many popular opinions based on various biases. Yet, while choosing the serverless there are many things that go on than the surface picture.

Considering how critical this is for long-term success, platforms should be evaluated based on their functionalities and service integrations. The potential backlash in choosing a serverless platform is high. Some of them are vendor lock-in and data lock-in, as Patrick Debois, CTO at Zender, mentions in his tweet.

So how do you select the right serverless provider? The answer is in this article. Here we discuss the major serverless provider considering their technical empowerment which will help you to equally weigh the selection process directed towards your specific needs.

### Current Version:

As of today, AWS Lambda vs Azure Functions vs Google Cloud Functions all are production ready and generally available. Reflecting over the timeline, AWS Lambda became publicly available in early 2015, Azure Functions in late 2016 while Google Cloud Functions, just recently, in July 2018.

With such a big difference in their timeline, it is evident that AWS Lambda dominates the *serverless landscape* with the huge developer community and the wide spectrum of use cases. Moreover, it has advanced functionalities like edge processing and chaining.

Azure Functions supports the variety of supported runtime environment. The current approach of Azure is to provide a functional IDE in their portal to help you prototype and deploy functions.

When it comes to picking anyone out of this, the prerequisites and the evaluation criteria that you’ll employ will be unique to your organization. However, there are some common areas that these serverless providers fall into. Let’s discuss some of them.

![image info](/img/awscloud/5/Dia1-5-1.png)

### Language Support & Deployment

AWS Lambda currently has native support for code written in JavaScript, Node.js, Python, Java and C#, along with releasing support for Golang earlier this year. These wide choices of runtime environments and its versatility makes it the first choice for developers.

AWS Lambda provides API operations that you can use to create and update Lambda functions with the help of a deployment package uploaded to the console in a ZIP file or edited within the console itself.

> Dependency management aside the biggest barrier to entry is language runtime support. Most FaaS platforms only support a handful of language runtimes. AWS Lambda and Azure Functions being the exceptions as both platforms support most popular language runtimes.— Kelsey Hightower (@kelseyhightower) February 26, 2018

On the other hand, Azure Function supports C#, F#, Python, Java, Node.js, Python & PHP. Contrary to AWS Lambda, Azure Functions provides you with multiple options for deploying your function, such as GitHub, DropBox, Visual Studio, Kudu Console, Zip deployment and One Drive.

With Google Cloud Functions, your function execution varies by your chosen language. Currently, it supports Node.js 6, Node.js 7 and Python 3.7 with more to be supported soon. For deployment, you can choose either of these: CLI, Zip upload, inline web editor, Cloud Storage and Cloud Storage Repositories.

Truple.io is currently using AWS Lambda with Serverless Framework. According to its cloud architect, they used Azure Functions for 6 months but didn’t have a good experience. Eventually, they abandoned Azure Functions and moved to  Kubernetes. Here why,

"*AWS Lambda had most of the problems solved already for Node.Js. It has good scalability without any resource issues. AWS Lambda, I feel it works as it is promised. However, with C#, I did use it on AWS Lambda the week after it was released. However, it had some issues that were solved after almost 8 months.*

*On the contrary, in Azure Functions, I was using C#  and we continually had issue after issue. Due to lack of documentation, we were constantly running into issues. Not only that, App Insights (monitoring platform for Azure Functions) took 5+ minutes to load the logs.*

*Apart from this, we recently faced issues with Azure CDN, where we were unable to purge files so old, outdated files continued to be served up for 16+ hours after the purge. Cloudfront in AWS invalidates works in less than 15 minutes every time I do it”*

### Dependencies Management
With AWS Lambda, dependencies are packaged within the deployment packages itself. To do it successfully, you first need to organize your code and dependencies in a certain way.

Guidelines for creating the deployment package varies depending upon your choice of language. You may also use build plugins such as Jenkins (for Node.js and Python) and Maven (Java) to create the packages.

With Azure Functions, You can include your package.json in your function directory and run npm install as you normally would with Node.js projects using Kudu or the Console in the Azure portal. This process is similar irrespective of your runtime language.

Google Cloud Functions let you manage your dependencies with npm and are expressed in a metadata file called package.json. Your function also has an option to downloads the dependencies declared in package.json using npm.

Google Cloud Functions install dependencies on behalf of the user, which means that they do not need to take awkward steps like “vendoring” them locally (required for AWS Lambda) and they also avoid potential pitfalls, e.g., if the developer is using a Windows machine and vendors a library with native extensions (C code), there’s a high probability that it will not work on AWS Lambda when deployed.

### Persistent Storage
The essence of serverless functions lies in its statelessness. Saying that an ideal function would be the one written in a stateless style which has no affinity to the underlying compute infrastructure or other variables.

With AWS Lambda, the function is required to be written without any variables. At the same time, you have an option for storage of variables in the persistent storage such as S3, DynamoDB or any other cloud storage services.

With Azure Functions to store the persistent data that needs to be constant across numerous instances, you may use Azure Blob Storage or Azure Table Storage.

If you want to “cache” data, for instance, a DB connection, you can use static variables. For Google Cloud Functions, if you require to share state across function invocations, your function should use services such as Cloud Datastore, Cloud Firestore, Cloud Storage and Cloud SQL to persist data.

### Identity & Access Management
For Identity and access management, IAM system provides you with an authorization layer that allows you to exercise fine-grained access management for your functions.

For example, what they can do with those resources (read-only or write-only access), and what areas they have access to (specific function or whole project).

AWS Lambda provides you with the facility to create your own custom IAM policies and attach them with your Lambda functions. This allows permissions for AWS Lambda API actions, users, groups, roles and resources.

While Azure Functions lets you control your function policies through Resource Based Access Control. It is supported at Subscription and ResourceGroup. Though at the moment, you can give permission to read/write access both to your functions as read-only access disables some of the app’s features.

However, with Google Cloud Functions’ IAM roles you can list the permissions that are contained in each role. Roles can be granted to users on an entire project or on individual functions. Though there are more features in Alpha version soon to be released publicly.

### Triggers & Types
Event sources (triggers) are custom events that invoke your serverless function. AWS Lambda gives you the feature for invocation over HTTPS defined by a custom REST API and an endpoint using API Gateway.

Other than that, AWS Lambda supports a plethora of other AWS service that you can configure as an event source. Lambdas can also be invoked through AWS SDKs given that it has the necessary permission for invocations.

With Azure Functions, there are few Microsoft services such as CosmosDB, Queue Storage, Service Bus and Table Storage. However, one advance feature is that external HTTP and Webhooks (from GitHub) can be used for invocation, along with APIM, function proxy and bindings.

Google Cloud Functions supports HTTP triggers provides that you need to specify while creating your function endpoint and these works only synchronously. Apart from this, Cloud Storage and Cloud Pub/Sub can work as event sources. There are more event sources though they are in Beta version: Firebase Authentication, Real-time Database, Analytics for Firebase and Cloud Firestore.

### Orchestration
Applications with serverless architecture, where functions start, execute and finish in a matter of milliseconds, do not preserve state. Each function is completely independent and data cannot be saved in the container, which is demolished when the function completes its task.

For enabling state in a stateless architecture and orchestrate functions, AWS created Step Functions. This module logs the state of each function so it can be used by subsequent functions or for root-cause analysis.

With Azure Functions, you can orchestrate and automate the tasks using Azure Logic Apps as well as Durable Functions. You can easily integrate different services of cloud and on-premise through connectors.  Google Cloud Functions at present doesn’t support integration with Cloud Composer, GCP’s workflow orchestration services.

Laurens Van Houtven, Principal at Latacora, faced quite a few problems with functions orchestration and monitoring. Here’s what he has to say,

“*With Google Cloud Functions, there were two major problems that we faced, first, there isn’t a straightforward way to integrate it with any mail services (like SES + AWS Lambda), secondly, it has limited runtime support for the function written in JS.*

*Other problems our clients have seen include e.g. limited orchestration support (step functions) and that API Gateway doesn’t scale down to very simple things.*

*They’re hard to monitor because most security monitoring tools don’t deal with them (AWS Config finally got Lambda support last week) but we solve that problem with internal tooling. The annoyances with Lambda (e.g. API Gateway) you can mostly work around by using e.g. Terraform to orchestrate which you should be doing anyway. The limitations with alternatives are pretty fundamental.*”

### Concurrency & Execution Time
Concurrency refers to the parallel number of executions that happens at any given time. You can estimate the concurrent execution count, but this count differs depending upon the type of event source.

Also, you functions scale automatically based on incoming request rate, but not all resources in your architecture may be able to do so. Hence, concurrency is also dependent on downstream resources support.

Currently, AWS Lambda limits the total concurrent executions across the functions within a given region to 1000. You can control concurrency in two ways, for individual functional level and account level. Also, the maximum function execution time is 900 seconds or 15 minutes.

At present, Azure functions support multiple functions concurrently provided they are within a single app instance which has no limit. The number of concurrent activity and orchestrator function executions is capped at 10X the number of cores on the VM. Like AWS Lambda, this also has an execution time limit of 300 seconds or 5 minutes which is upgradeable to 600 seconds or 10 minutes within consumption plan.

Likewise, with Google Cloud Functions, there is no concurrent limit for HTTP invocation. For other types of invocation, the maximum number of concurrent function requests is 1000. However, the maximum execution time, unlike other service providers, is 60 seconds by default which can be upgraded to 540 seconds or 9 minutes.

*Note: An important difference between AWS and Google Cloud Functions is that concurrency is measured at the account level for AWS but at the project level for Cloud Functions*.

This means that, for AWS, if a function executes, the total number of available executions for all functions is reduced by 1 for the account. For Google Cloud Functions, if a function executes, the number of available executions for only that function is reduced by 1 and not for the account.

In theory, you could have 10 functions, each with 1,000 concurrent executions on Cloud Functions whereas you could only have 1 such function on AWS.

### Scalability & Availability
AWS Lambda supports dynamic scalability in response to the increased traffic. However, this is subjected to an individual account level concurrent execution limit.

To manage traffic burst, AWS Lambda will immediately increase your concurrently executing functions by a predetermined amount, dependent on which region it’s executed. If there is no predefined limit, it keeps on increasing the number of functions by 500 per minute until your demand is met.

Currently, Azure Functions are available based upon two different plans. First, the consumption plan which scales your function automatically where a function execution times out after a configurable period of time. Second, the app service plan which runs your functions on the dedicated VMs.

Dedicated VMs are allocated to every function app, which means the function’s host can be always up and running.

With Google Cloud Functions, you have the novelty of automatic scalability. However, the background functions scale more gradually and it depends on the duration of functions and longer functions will scale slightly slower. Also, maximum scalability is based upon the rate limits.

### Logging & Monitoring
AWS Lambda has inbuilt functionality of monitoring Lambda functions by reporting metrics through Amazon CloudWatch. These metrics include a number of requests, latency per request, number of requests resulting in an error.

It automatically integrates with Amazon CloudWatch Logs and pushes all logs from your code to a CloudWatch Logs group associated with a Lambda function. You can also use AWS X-Ray to leverage end-to-end monitoring of your functions, however, it comes with the extra price.

Azure Functions comes with built-in integration with Azure Application Insights for monitoring functions. Functions also have built-in monitoring that doesn’t use Application Insights. If you do not want to use Application Insights, you can opt for a built-in logging system, however, that uses Azure Storage.

Google Cloud Platform provides you with Google Stackdriver which is a suite of monitoring tools (Stackdriver Logging, Stackdriver Error Reporting, Stackdriver Monitoring) that helps you understand what’s going on in your Cloud Functions. It has in-built tools for logging, reporting errors and monitoring. Apart from Stackdriver, you can view execution times, execution counts and memory usage in the GCP Console.

### AWS Lambda vs Azure Functions vs Google Cloud Functions: Pricing Comparison
Before we jump on to the exact dollars, let’s understand the execution environment in a better manner.

Execution time is directly proportional to the execution environment. For example, if you’re dealing with a function which is computation heavy and takes a few milliseconds of time to execute. This might prompt you to opt for lower memory sizes as it will cost you less but it is a double-edged sword. The smaller the machine, the more time it will take to execute a function.

AWS Lambda has a free-tier under which it covers first 1M function requests & 400,000 GB-secs per month. Thereafter, you will be required to pay $0.20 per 1M requests or $0.0000002 per request. Also, $0.00001667 for every GB-secs used.

Azure Functions’ free tier covers 1M requests and 400,000 GB-secs on the monthly basis. Afterwards, you will pay $0.000016/GB-secs and $0.20 per 1M executions.

Google Cloud Functions are priced on the execution time (GB-sec), the number of invocations and how many resources you provision for the function (GHz-secs).

The free tier includes first 2M requests and $0.40 beyond that. Compute time is measured in two units: GB-secs & GHz-secs. Moreover, networking outbound data is charged flatly at $0.12/Gb with first 5GB free. This pricing model makes it a little different than the rest of the contenders.

*Note: One another important pricing factor we shouldn’t ignore is API Gateway. For example, with AWS Lambda, API Gateway is a prerequisite and is required for HTTP & HTTPS invocations both. However, with Google Cloud Functions, HTTP requests don’t require an API Gateway.*

This is one of the major benefits of Google Cloud Functions: HTTP functions do not require an API Gateway. For AWS, this contributes a significant amount of cost and complexity in running your function. With Google Cloud Functions, you get an HTTP endpoint and routing/networking are set up for you automatically. You don’t need to pay an additional cost to an API Gateway.

Read more: For a detailed analysis of serverless pricing refer to this blog. If you’re keen on what it takes to run serverless applications, here is an in-depth resource.

### AWS Lambda vs Azure Functions vs Google Cloud Functions: Performance Comparison
Recently at USENIX Annual Technical Conference 2018, an academic report was published about the performance of various serverless platforms. The research was conducted on a single “measurement function” which performed the two basic tasks:

* Collect invocation timing and runtime information of function instance
* Run specified tasks (measuring local disk I/O throughput, network throughput) based on received messages.
  
**#1. Concurrency & Scalability**

For AWS, 3328 MB was the maximum aggregate memory that can be allocated across all function instances on any VM in AWS Lambda. This perhaps suggests that AWS Lambda tries to place a new function instance on the top of the existing VM so as to increase the memory utilization rates.

Despite Azure Functions claiming to scale 200 instances for a single Node.js function, it fails to do in practice. In this experiment, it was able to scale at most 10 function instances running concurrently for a single function.

With Google Cloud Functions, only half of the expected number of instances, even for a low concurrency level, could be launched at the same time. The remaining requests were queued.

**#2. Cold Start**

Speaking of AWS Lambda, there wasn’t any major cold start difference between the launching of existing and new instance. In fact, the median cold start time i.e. latency between the two cases was only 39 ms.

On the contrary, it took a long time for the Azure instances to launch despite the fact that 1.5 GB memory was allocated to them. Median cold start latency was observed at 3640 ms in Azure.

Surprisingly, with Google Cloud Functions, the median cold start latency ranged from 110 ms to 493 ms. At present, it allocated memory proportionally to CPU, but in Google, memory size has more impact on cold start latency than in AWS.

![image info](/img/awscloud/5/academic2-1-610x467.jpg)

**#3. Instance Maximum Idle Time**

This can be loosely defined as the maximum time an instance can stay idle before it shuts down. Here are the results:

For AWS, an instance was able to stay active for 27 minutes. Notably, in 80% of the cases, instances were shut down after 26 minutes. With Azure Functions, no consistent idle time was discovered.

For Google Cloud Functions, the idle time was as huge as 120 minutes. Even after 120 minutes, in 18% of the cases, the instances remained active.

However, this data may contradict when you put your specific needs in practice. Not only that, but the platform’s internal security practices also play a major role.

Recently I came across an opinion where a company had to move a large SaaS application from AWS to Azure. The tech stack included 80% Linux/OSS and 20% .NET.

The organization switched to Azure due to its affinity for security compliance and data sovereignty. After the switch, they came to realize that Azure is much stronger in support and in an ability to interact with Azure product teams to help guide the direction of the platform.

It is evident from the recent reports Azure is keeping up with the face in the serverless landscape and there is no way to neglect them.

### Conclusion
According to the latest study by CNCF, AWS leads the serverless race with almost 70% of the production deployment done using AWS Lambda.

> AWS Lambda owns 70% percent of the active serverless platform user base – https://t.co/oTqy1sN1Wm … let me translate that for you. Amazon is currently positioned to own 70% of the future of ALL software.— Simon Wardley #Labour #EEA #Exit #Attlee #Disraeli (@swardley) April 19, 2018

The 2-year head start (launched in 2014), quick implementation of new features and performance are the major factors in AWS Lambda’s pole position.

Presently serverless is driven by cloud-native practitioners, but as serverless adoption increases the organizations will continue to utilize multiple clouds, with AWS usually being part of the mix. Thus in the future, we are likely to see Azure and Google Cloud Functions catching up with AWS Lambda.

If you are diving into serverless with no platform preference, I would encourage everyone to explore and try each of these before incorporating them into their custom software development services. Understand the use cases why and when serverless can help you.