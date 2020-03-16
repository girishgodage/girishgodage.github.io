---
title: Cloud Cost Optimization Strategies (Even AWS won't Tell you!)
date: 2020-01-25 11:41:00 Z
permalink: "/blog/cloud-cost-optimization-strategies"
categories:
- AWSCloud
tags:
- learning
summary: Fine-tuning your cloud infrastructure is critical to make sure that your overall bill keeps up to its limit. Read this blog to find out proven cloud cost optimization strategies to help you cut down on the bill and save money by eliminating unused resources.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-25-cloud-cost-optimization-strategies
---

## 6 Cloud Cost Optimization Strategies (Even AWS won't Tell you!)

Fine-tuning your cloud infrastructure is critical to make sure that your overall bill keeps up to its limit. Read this blog to find out proven cloud cost optimization strategies to help you cut down on the bill and save money by eliminating unused resources.

![image info](/img/awscloud/16/Diagram1.png)

Stanford University researcher and lecturer Dr. Jonathan Koomey presented a report last year which stated a surprising fact that only 20 per cent of organizations would save money if they transferred their server data directly to the cloud, whereas 80 per cent would see an increase in the annual costs.

For those organizations comprising of 80 per cent, the extra cost rolls over when they move on to the cloud, Koomey found, which results in organizations paying an average of 36 per cent more of cloud services than they actually need to. Given Gartner's projection that data storage will be a $173 billion business in 2018, companies globally could save $62 billion on storage just by optimizing their workloads. This is understandable since, for organizations using cloud resources aggressively, it becomes difficult to keep the cloud bills in check.

If your organization is one of those who overspend on the cloud resources, this blog is for you where we discuss some of the prominent strategies that I put into practice so that our clients get the best result in the lease possible spending.

For any organization, the major spend of 70-80 per cent comprises of the storage and compute resources. And hence, this makes it inherently important to monitor these aspects closely. However, the first step towards keeping a strict eye on the bill is to increase the visibility into the cloud costs and understanding the total cost of ownership. Let's get started!

* ## Cost Optimization Strategies for compute
* ## Cost Optimization Strategies for Storage
* ## Cost Optimization Tools
  


## AWS Cloud Cost Optimization Strategies

The majority of spending for most users is on compute, making it a key area of focus for cost reductions.

### 1. Downsize under-utilized instances
Downsizing one size in an instance family reduces costs by 50 per cent. Let's take one example of m4.large and t2.small.

![image info](/img/awscloud/16/Scale.png)

As shown in the image, m4.large is utilized less than 50% between 0 to 9 am. Server usage increases from 9 am to 2 pm. Here it requires two m4.large instances. Again, usage starts decreasing till 6 pm and increases in the night. As per the graph, it is clear that during some specific time span single m4.large can't be utilized even 50% of its capacity. It costs $4.1/day.

Under-utilized instances should be considered as candidates for downsizing either one or two instance sizes. Keep in mind that downsizing one size in an instance family reduces costs by 50 per cent.

According to server load graph, we can use t2.small for utmost utilization. As t2.small has a half compute capacity than m4.large so 70 t2.small instances are required instead of 41 m4.large instances. Using t2.small instances the decision makers can save more than 60% of the instance cost.

### 2. Turn off unused instances
Organizations use instances according to the highest peak of requirement which is not constant. Due to variation in the requirement, organizations have to pay for non-utilized instances. To fully optimize cloud spend, turn instances off that are no longer being used. The below image shows how servers load varies on a particular day and then turn off instances.


