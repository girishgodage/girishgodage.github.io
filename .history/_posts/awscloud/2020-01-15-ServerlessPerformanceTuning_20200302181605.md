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

 