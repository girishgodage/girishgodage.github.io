---
title: Serverless Performance Tuning with AWS Lambda
date: 2020-01-15 10:41:00 Z
permalink: "/blog/serverless-performance"
categories:
- AWSCloud
tags:
- learning
summary: Efficient configuration of AWS Lambda functions is highly critical when you're expecting an optimal performance of your serverless applications. This blog discusses potential serverless performance bottlenecks and ways through which you can finetune AWS Lambda performance.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-15-serverless-performance
---

##  Serverless Performance Tuning

  Efficient configuration of AWS Lambda functions is highly critical when you're expecting an optimal performance of your serverless applications. This blog discusses potential serverless performance bottlenecks and ways through which you can finetune AWS Lambda performance.

 ![image info](/img/awscloud/7/serverless-performance-1024x555.png)

 We’ve seen some remarkable and exponential client growth. Months over months, the number of projects that we are handling is growing at an exponential rate. And so is the expectation of exceptional service.

As we move into this article, you’ll discover that predicting the performance of Serverless systems is quite a difficult job, especially for lower-memory functions. And hence, in this blog, we’ll try to set some concrete benchmarks which you can use to configure the performance of your serverless system.

### AWS Lambda Performance Benchmark 2019

* Optimal Memory Size
* Language Runtime
* Parallel Version Invocation
* Concurrency Issues
* Cold vs Hot Startups
* Code Optimization for Efficient Performance

###  Optimal memory size
Well, with some serverless vendors, you have the limit for choosing the memory from 128 MB to 1308 MB while some vendors select the memory automatically according to your function.

This leaves the question of how to choose the optimal memory size for your functions. What I have observed is that simply choosing the memory size that sufficiently runs your function isn’t going to work. In fact, that’s an anti-pattern. Here’s why!

Let’s take the example of AWS Lambda. Lambda provides you with a single dial to allocate the number of computing resources (RAM) to your function. This RAM allocation also impacts the amount of CPU time and network bandwidth received by your function.

Now, why is it an anti-pattern? Because Lambda billing is as accurate as 100-ms increments. Hence, opting for the smallest sufficient amount of RAM might add latency to your application. If the increase in latency outweighs the resource cost savings, it might come off as more expensive.

Well, the noteworthy point here is that you should test your lambda functions at all available resources level so as to determine the excellent level of price/performance for your application. You will observe that the performance level of your functions will increase logarithmically over time.

**System Specification**

The limits that you’ll set initially will be working as base limits. As you move on, you’ll come across a resource threshold where any additional RAM/CPU/Bandwidth available to your functions no longer provides any significant performance gain.

However, the prices of lambda functions increase linearly with the increase in computing resources. And that is why your test should be able to determine the logarithmic function bend to choose the excellent configuration of your function.

**Performance Results**

Here’s the experiment was done by AWS which clearly shows that ideal memory allocation can be cost-effective with lower latency as well. Moreover, the additional cost of computing per 100 ms for using 512 MB over the lower memory options is far better than the amount of latency reduced in the function by allocating more resources.

![image info](/img/awscloud/7/121.png)