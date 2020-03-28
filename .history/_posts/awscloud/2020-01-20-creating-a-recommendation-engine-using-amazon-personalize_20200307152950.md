---
title: Creating a recommendation engine using Amazon Personalize
date: 2020-01-20 10:41:00 Z
permalink: "/blog/creating-a-recommendation-engine-using-amazon-personalize"
categories:
- AWSCloud
tags:
- learning
summary: AWS announced Amazon Personalize, which allows you to get your first recommendation
  engine running quickly, to deliver immediate value to your end user or business.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-20-AmzonPersonalize
---

## Amazon Personalize - Creating a recommendation engine

AWS announced **Amazon Personalize**, which allows you to get your first recommendation engine running quickly, to deliver immediate value to your end user or business. As your understanding increases (or if you are already familiar with data science), you can take advantage of the deep capabilities of Amazon Personalize to improve your recommendations.

While at working , I've noticed increasing diversity in the application of machine learning (ML) and deep learning. It seems that nearly every day I work on a new exciting use case, which is great!

The most well-known and successful ML use cases have been **retail websites, music streaming apps, and social media platforms**. For years, they've been embedding ML technologies into the heart of their user experience. They commonly provide each user with an individual personalized recommendation, based on both historic data points and real-time activity (such as click data).

I was lucky enough to be given early access to try out Amazon Personalize while it was in preview release. Instead of giving it to data scientists or data engineers, the company gave it to me, an AWS solutions architect. With no prior knowledge, I was able to get a recommendation from Amazon Personalize in just a few hours. This post describes how I did so.

## Overview
The most daunting aspect of building a recommendation engine is knowing where to start. This is even more difficult when you have limited or little experience with ML. However, you may be lucky enough to know what you don't know (and what you should figure out), such as:

* **What data to use.**
* **How to structure it.**
* **What framework/recipe is needed.**
* **How to train it with data.**
* **How to know if it's accurate.**
* **How to use it within a real-time application.**

Basically, Amazon Personalize provides a structure and supports you as it guides you through these topics. Or, if you're a data scientist, it can act as an accelerator for your own implementation.

### Creating an Amazon Personalize recommendation solution

You can create your own custom Amazon Personalize recommendation solution in a few hours. Work through the process in the following diagram.

![image info](/img/awscloud/9/arch-1.gif)