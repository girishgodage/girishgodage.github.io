---
title: Azure App Service Overview
date: 2020-01-07 10:41:00 Z
permalink: "/blog/azure-app-service"
categories:
- AzureCloud
tags:
- learning
summary: Azure App Service enables you to build and host web apps, mobile back ends, and RESTful APIs in the programming language of your choice without managing infrastructure. It offers auto-scaling and high availability, supports both Windows and Linux, and enables automated deployments from GitHub, Azure DevOps, or any Git repo. Learn how to use Azure App Service. 
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-azure-app-service
---

## Azure App Service

 Azure App Service enables you to **build and host web apps, mobile back ends, and RESTful APIs** in the **programming language of your choice without managing infrastructure**. It offers **auto-scaling and high availability**, supports both **Windows and Linux**, and **enables automated deployments from GitHub, Azure DevOps, or any Git repo**. Learn how to use Azure App Service.

## Overview

 It is an **HTTP-based service** for hosting web applications, REST APIs, and mobile back ends. You can develop in your favorite language, be it **.NET, .NET Core, Java, Ruby, Node.js, PHP, or Python**. Applications run and scale with ease on both Windows and Linux-based environments. 

 App Service not only **adds the power of Microsoft Azure** to your application but you can also take advantage of its **DevOps capabilities**.

> **Power of Microsoft Azure**, such as security, load  balancing, autoscaling, and automated management.
>
> **DevOps capabilities**, such as **continuous deployment** from *Azure DevOps, GitHub, Docker Hub, and other sources*, **package management**, **staging environments**, **custom domain**, and **SSL certificates**

With **App Service**, **you pay for the Azure compute resources you use**. The compute resources you use is determined by the App Service plan that you run your apps on.

## Why use App Service?

Here are some key features of App Service:

* **Multiple languages and frameworks** - App Service has first-class support for ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP, or Python. You can also **run PowerShell and other scripts or executables as background services**.
  
* **DevOps optimization** - Set up **continuous integration and deployment** with Azure DevOps, GitHub, BitBucket, Docker Hub, or Azure Container Registry. Promote updates through **test and staging environments**. Manage your apps in App Service by using **Azure PowerShell** or the **cross-platform command-line interface (CLI)**.
* **Global scale with high availability** - Scale **up** or **out** manually or automatically. Host your apps anywhere in Microsoft's global datacenter infrastructure, and the App Service **SLA** promises high availability.
* **Connections to SaaS platforms and on-premises data** - Choose from more than 50 **connectors** for enterprise systems (such as SAP), SaaS services (such as Salesforce), and internet services (such as Facebook). Access on-premises data using **Hybrid Connections** and **Azure Virtual Networks**.
* **Security and compliance** - App Service is **ISO, SOC, and PCI compliant**. Authenticate users with **Azure Active Directory** or with social login (**Google**, **Facebook**, **Twitter**, and **Microsoft**). Create **IP address restrictions** and **manage service identities**.
* **Application templates** - Choose from an extensive list of application templates in the **Azure Marketplace**, such as WordPress, Joomla, and Drupal.
* **Visual Studio integration** - Dedicated tools in Visual Studio streamline the work of creating, deploying, and debugging.
* **API and mobile features** - App Service provides turn-key CORS support for RESTful API scenarios, and simplifies mobile app scenarios by enabling authentication, offline data sync, push notifications, and more.
* **Serverless code** - Run a code snippet or script on-demand without having to explicitly provision or manage infrastructure, and pay only for the compute time your code actually uses (see [Azure Functions](/azure/azure-functions/)).

Besides App Service, Azure offers other services that can be used for hosting websites and web applications. For most scenarios, App Service is the best choice.  For microservice architecture, consider **Service Fabric**. If you need more control over the VMs that your code runs on, consider **Azure Virtual Machines**.