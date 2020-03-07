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

I've noticed increasing diversity in the application of machine learning (ML) and deep learning. It seems that nearly every day I work on a new exciting use case, which is great!

The most well-known and successful ML use cases have been **retail websites, music streaming apps, and social media platforms**. For years, they've been embedding ML technologies into the heart of their user experience. They commonly provide each user with an individual personalized recommendation, based on both historic data points and real-time activity (such as click data).

I was lucky enough to be given early access to try out Amazon Personalize while it was in preview release. Instead of giving it to data scientists or data engineers, the company gave it to me, an AWS solutions architect. With no prior knowledge, I was able to get a recommendation from Amazon Personalize in just a few hours. This post describes how I did so.
