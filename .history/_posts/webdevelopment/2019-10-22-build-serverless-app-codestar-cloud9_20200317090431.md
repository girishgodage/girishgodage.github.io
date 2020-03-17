---
title: Build a Serverless Application with AWS CodeStar and AWS Cloud9
date: 2019-10-22 10:41:00 Z
permalink: "/blog/build-serverless-app-codestar-cloud9"
categories:
- Web Development
tags:
- learning
summary: you will learn how to use AWS CodeStar and AWS Cloud9 to develop, build, and deploy a Node.js serverless web application. As a developer, setting up an automated software development workflow can be a time-intensive, detailed task. AWS CodeStar is a software development tool that enables you to quickly develop, build, and deploy applications on AWS. With CodeStar, you can setup your continuous delivery toolchain in minutes, allowing you to start releasing code faster..
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2019-10-22-build-serverless-app-codestar-cloud9
---

## Build a Serverless Application with AWS CodeStar and AWS Cloud9

 You will learn how to use AWS CodeStar and AWS Cloud9 to develop, build, and deploy a Node.js serverless web application. 
 
 As a developer, setting up an automated software development workflow can be a time-intensive, detailed task. 
 
 **AWS CodeStar is a software development tool** that enables you to quickly develop, build, and deploy applications on AWS. With CodeStar, you can setup your continuous delivery toolchain in minutes, allowing you to start releasing code faster.

 Cloud9 is a cloud IDE for writing, running, and debugging code. Cloud9 comes prepackaged with essential tools for many popular programming languages **(JavaScript, Python, PHP, etc.)** so you don't have to tinker with installing various compilers and toolchains.

In the next several minutes, you'll use AWS CodeStar to build a new AWS Lambda based Node.js serverless web application. You will use AWS CodeStar to set up a continuous delivery toolchain using AWS CodeCommit for source control and AWS CodePipeline to automate your release process. You will then change some code in the Node.js project using Cloud9 and commit the change to trigger your continuous pipeline and redeploy your project.

***The AWS services you use in this tutorial are within the AWS Free Tier.***

## Step 1: Navigate to CodeStar
Open the **AWS Management Console**, so you can keep this step-by-step guide open. When the screen loads, enter your user name and password to get started. Then type Codestar in the search bar and select CodeStar to open the CodeStar console.

![image info](/img/webdevelopment/5/build-serverless-app-codestar-cloud.png)

## Step 2: Build a simple NodeJS app in CodeStar

In this step, you will set up CodeStar then create and deploy a serverless AWS Lambda Node.js project.

a. On the CodeStar home Page, select Start a project.

![image info](/img/webdevelopment/5/build-serverless-app-codestar-cloud9-01.png)

b. CodeStar can assist you by administering AWS resources on your behalf; to enable this feature, CodeStar needs to create an AWS service role for you. On the **Create service role** dialog, choose **`Yes, create role**.

![image info](/img/webdevelopment/5/build-serverless-app-codestar-cloud9-02.png)

c. On the **Choose a project template** page, choose the Node.js template which includes a Web application and AWS Lambda. You can use CodeStar to develop a variety of application such as websites, web applications, web services, and Alexa skills. You can develop in Java, JavaScript, PHP, Ruby, C#, Go, HTML, and Python.

![image info](/img/webdevelopment/5/build-serverless-app-codestar-cloud9-03.png)
