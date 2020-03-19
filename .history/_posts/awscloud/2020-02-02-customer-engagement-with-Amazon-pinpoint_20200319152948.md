---
title: Amazon pinpoint - Customer Engagement
date: 2020-02-02 11:41:00 Z
permalink: "/blog/customer-engagement-with-Amazon-pinpoint"
categories:
- AWSCloud
tags:
- learning
summary: Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels. You can use Amazon Pinpoint to send push notifications, emails, SMS text messages, and voice messages.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-02-02-customer-engagement-with-Amazon-pinpoint

---

## What is Customer Engagement?

Customer engagement refers to the practices that organizations employ to keep users coming back to their applications. Maximizing customer engagement is vital in today's digital marketplace, where an ever-increasing number of brands are competing for a finite amount of customer attention. In this article, we discuss the factors that impact customer engagement, and provide information about specific steps you can take to improve customer engagement.

### Why customer engagement matters
In 2012, the average adult in the United States spent one hour per day using his or her mobile device; by 2017, that figure had increased to 5.6 hours per day[1]. Over half of that time is spent using third-party apps, but the majority of app users access 20 or fewer apps each month. Of those 20 apps, most users spent the vast majority of their time in their top 10 applications[2].

At the same time, first impressions are everything in today's digital marketplace. Almost a quarter of users have downloaded an app, ran it once, and never opened it again[3]. Additional research suggests that as many as 80% of all app users stop using a given app within 90 days[4].

These statistics paint a very clear picture: in order to succeed in this highly competitive environment, organizations must deliver digital content that delights and engages their customers.

### Customer engagement in action
Let's assume your organization produces a mobile app for sports fans called All Things Sports. In your app, users can get the latest sports news, purchase memorabilia and event tickets, and play exciting mini-games. Your organization has devoted a large amount of time, effort, and money into creating a best-in-class app and acquiring new users. For this reason, it's essential that your users keep coming back to your application so that your business can grow.

For all applications, user interactions fall into three categories: activation, retention, and conversion. All Things Sports requires users to confirm their email address and select their favorite sports teams; the percentage of users who complete this process is the activation rate. Next, you need to offer content that is compelling enough to keep your users coming back. All Things Sports offers fun mini-games and special offers that encourage users to return to the app often. The likelihood that your customers will return to your application repeatedly is referred to as your retention rate. Finally, your organization needs to make money in order to grow. The main revenue generating activity in All Things Sports is the sale of event tickets and merchandise. The percentage of users who purchase these products through your app is your conversion rate. As the application developer, you have to continuously improve all three of these metrics in order to grow your business.

The first step toward improving these metrics is to gather customer analytics data. Customer analytics data is the data you collect and analyze to better understand the ways in which customers use your application. A thorough analysis of this data can help you better serve your users, thereby increasing their engagement with your application. Making optimizations to your application based on this data can lead to positive reviews, better app store rankings, and increased word of mouth. These factors can increase your activation rate, which can in turn increase your retention and conversion rates. You can start gathering this information by integrating the AWS Mobile SDK into your application. For more information about customer analytics, see our companion article, What are Customer Analytics?

The next step toward improving customer engagement is to create targeted segments of customers based on their characteristics. By targeting specific customers based on their demographics, preferences, behaviors, and interests, you can ensure that you're using the right channels to send the right messages to the right customers. Amazon Pinpoint can import app usage data obtained from the AWS Mobile SDK and use it to create customer segments. To learn more about the ways in which you can create customer segments, see our companion article, What is Customer Segmentation?

The final step toward improving customer engagement is to deploy personalized, multi-channel messaging to your customers. You can use these messages to proactively update engaged customers on exciting new features and improvements in your application, or to incentivize less-engaged customers to sign back in to your application. You can also use this messaging to convey vital transactional content—such as password reset emails or purchase confirmations—to all customers. You can also use Amazon Pinpoint to meet this need. For more information about multi-channel messaging, see the companion article titled What is Multi-Channel Messaging?
