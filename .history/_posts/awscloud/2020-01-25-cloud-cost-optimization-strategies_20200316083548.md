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

![image info](/img/awscloud/16/Scale-2.png)

As you can see in the above image, server load can be handled by 1 instance so you can turn off 2nd instance to save the cost. During the next 8 hours server load increases and requires two instances. Again, server load decreases from 16 to 24 and you can turn off 2nd instance.

Production instances should be auto-scaled to meet demand. Shutting down development and test instances in the evenings and on weekends when developers are no longer working can save 65 per cent or more of costs. And instances for training, demos, and development must be terminated once projects are completed.

### 3. Use discounted instances
It is very important to understand Discount options of Instances. In detail for AWS. With AWS Reserved Instances, you get the discount in exchange for making a one-year or three-year commitment with the longer commitment giving a higher discount. Discounts range from 24 to 75 per cent depending on the RI term, the instance type, and the region.

### 4. AWS spot instances
On the AWS spot market, the traded products are EC2 instances, and they're delivered by starting an EC2 instance. It is very common to see 15-60% savings, and in some cases, you can save up to 90% with spot instances.

**How does it work?**

You set a maximal bid price and optionally a period up to 6 hours. The price you pay is the spot price each hour. When this price goes above your specified maximum bid price, the instance is terminated. Be careful that the new console for spot creates you a fleet, and even terminating the instance yourself, the fleet remains open. You need to cancel the "spot request" (the fleet), as well as terminate the instance(s) when you are done. The instances are guaranteed to remain active for the specified amount of time up to 6 hours. See below image for more understanding.

![image info](/img/awscloud/16/Scale-2-1.png)


Cloud Cost Optimization Strategies
You may also like: How to Choose the Right Cloud Deployment Model for your Application?

There are many different spot markets available. A spot market is defined by:

Instance type (e.g. m3.medium)
Region (e.g. eu-west-1)
Availability Zone (e.g. eu-west-1a)
Each spot market is offering a separate current price. So when using spot instances it is an advantage to be able to use different instance types, in different availability zones or even regions, as this allows you to pick the lowest price available.

5. Minimizing data transfer costs
Make sure your Object Storage and Compute Services in the same region because Data transfer is free in the same region. For example, AWS charges $0.02/GB to download the file from another AWS region.

If you do a lot of cross-region transfers it may be cheaper to replicate your Object Storage bucket to a different region than download each between regions each time. Let's understand it with AWS S3's example.

1GB data in us-west-2 is anticipated to be transferred 20 times to EC2 in us-east-1. If you initiate inter-region transfer, you will pay $0.20 for data transfer (20 * 0.02). However, if you first download it to mirror S3 bucket in us-east-1 then you just pay $0.02 for transfer and $0.03 for storage over a month. It is 75% cheaper. This feature is built into S3 called cross-region replication. You will also get better performance along with cost benefit.

Use AWS content delivery network called CloudFront If there are a lot of downloads from the servers which are stored in S3 (e.g. images on consumer site).

There are CDN providers such as CloudFlare who charge a flat fee. If you have a lot of static assets then CDN can give huge savings over S3, as just a tiny per cent of original requests will hit your S3 bucket.

6. Delete or migrate unwanted files after a certain date
Cloud architects can programmatically configure rules for data deletion or migration between types of storage. This drastically reduces the long-term storage costs. All the major cloud providers have the feature of Lifecycle Management. For instance, active data can remain in Azure Blob Standard storage. But, if certain data begin to show signs of infrequent access, users can program rules to migrate that data over Azure Cool Blob storage to incur a cheaper storage rate.

A lot of deployments uses Object Storage for log collection. You may automate deletion using life cycles. Delete objects 7 days after their creation time. E.g. if you use S3 for backups, it makes sense to delete them after a year. Similarly, you can use the Object Lifecycle management feature in Google cloud and expiration of Blobs in Azure.

7. Compress Data Before Storage
Compressing data reduces your storage requirements. Subsequently reducing the cost of storage. Using fast compression algorithm such as LZ4 gives better performance. LZ4 is a lossless compression algorithm, providing compression speed at 400 MB/s per core (0.16 Bytes/cycle). It features an extremely fast decoder, with speed in multiple GB/s per core (0.71 Bytes/cycle). In many use cases, it makes sense to use compute-intensive compressions such as GZIP or ZSTD.

8. Take care of incomplete multipart uploads
There are many partial objects in Object Storage which had been interrupted while uploading. If you have a petabyte Object Storage bucket, then even 1% of incomplete uploads may end up wasting terabytes of space. You should clean up incomplete uploads after 7 days.

9. Reduce cost on API access
Cost on API access can be reduced by using batch objects and avoiding a large number of small files. Since API calls are charged per object, regardless of its size. Uploading 1-byte costs the same as uploading 1GB. So usually small objects can cause API costs to soar.

For example in AWS S3, PUT calls cost $0.005/1000 calls. So to upload a 10GB file in 5MB chunks it will cost roughly $0.01. Whereas, for 10KB file chunks, it will cost approximately $5.00.

Similarly, it is a bad practice to use CALL/PUT options with tiny files using S3. It makes sense to use batch objects or a database such as DynamoDB or MySQL instead of S3. 1 writes per second in DynamoDB costs $0.00065/hour. Assuming 80% utilization, DynamoDB will cost $0.000226/1000 calls. This is 95% cheaper to use DynamoDB compared to AWS S3.

The S3 file names are not a database. Relying too much on S3 LIST calls is not the right design and using a proper database can typically be 10-20 times cheaper.

10. Design workloads for scalability
For any public cloud, scalability is a critical aspect. Scalability uses event-driven compute instances like AWS Lambda or containers like Google Container Engine to scale core services for important workloads, such as microservices. These techniques are intended to utilize more computing when it is needed. Once the requirement increases, those additional resources are released for reuse.

11. Cache storage strategically
Use memory-based caching, such as AWS ElastiCache. Caching improves data accessibility by moving important or frequently accessed in-memory instead of retrieving data from storage instances. This can reduce the expense of higher-tier cloud storage and improve the performance of some applications.

It gives best results for performance-sensitive workloads which run in remote regions or when efficient replication is needed for resilience.

12. Use auto-scaling to reduce costs during off hours
Most apps have busier and slower periods throughout a week or day. Take advantage of auto-scaling to save some money during slow periods.

These deployment types all support auto-scaling:

Cloud Services
App Services
VM Scale Sets (Including Batch, Service Fabric, Container Service)
Scaling could also mean shutting your app down completely. You can use "AlwaysOn" feature provides by App Services that controls if the app should shut down due to no activity. You could also schedule shutting down your dev/QA servers with something like DevTest Labs.
