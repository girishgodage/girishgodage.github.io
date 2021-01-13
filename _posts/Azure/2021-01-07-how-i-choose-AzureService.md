---
title: How I choose which services to use in Azure
date: 2021-01-07 09:55:00 Z
permalink: "/blog/how-i-choose-AzureService"
categories:
- AzureCloud
tags:
- learning
summary: In this article, we will show you the process which I used to choose the right service for running the application in Azure and storing the data in Azure cloud service.
image: "/img/azure/12/img1.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2021-01-07-how-i-choose-AzureService
---

## Introduction

Microsoft Azure is huge and changes fast! I’m impressed by the services and capabilities offered in Azure and by how quickly Microsoft releases new services and features. It can be overwhelming. There is so much out there — and the list continues to grow — it is sometimes hard to know which services to use for a given scenario.

I create Azure solutions for my customers, and I have a **method that I use to help me pick the right services**. This method helps me narrow down the services to choose from and pick the right ones for my solution. It helps me decide how to implement high-level requirements such as **“Running my application in Azure”** or **“Storing data for my application in Azure.”** 

![image info](/img/azure/12/img1.png)

Of course, these are just examples. There are many other categories to address when I’m architecting an Azure solution.

### A look at the process

Let me show you the process that I use for **“Running my application in Azure.”**


First, I try to answer several high-level questions, which in this case would be:

![image info](/img/azure/12/app_process.png)

  * **1. How much control do I need?**
  * **2. Where do I need my app to run?**
  * **3. What usage model do I need?**
  * **4. Which funcationality do I need?**

Once I’ve answered these questions, I’ve narrowed down the services from which to choose. And then, I look deeper into the services to see which one best matches the requirements of my application, including functionality as well as availability, performance, and costs.

Let’s go through the first part of the process where I answer the high-level questions about the category.

## Question 1: How much control do I need?

In considering how much control I need, I try to figure out the degree of control I need over the **operating system, load balancers, infrastructure, application scaling,** and so on. This decides the category of services that I will be selecting from.

On the control side of the spectrum is **infrastructure-as-a-service (IaaS)** category, which includes services like [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/) and [Azure Container Instances](https://azure.microsoft.com/services/container-instances/). These give a lot of control over the operating system and the infrastructure that runs your application. But with <span style="color:yellow;background-color:black;">**control comes great responsibility**</span>. For instance, if you control the operating system, you are responsible to update it and make sure that it is secure.

![image info](/img/azure/12/How_much_control.png)
*Figure 1. How much control do I need?*


Further up the stack are services that fall into the **platform-as-a-service (PaaS)** category, which contains services like [Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/). In PaaS, you don’t have control over the operating system that your application runs on, nor are you responsible for it. You do have control over scaling your application and your application configuration (e.g., the version of .NET you want your application to run on.

The next abstraction level is what I will here call **logic as a service (LaaS)**, also known as **serverless**. This category contains services like [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/). Here, Azure takes care of the underlying infrastructure for you, including scaling your app. Logic as a service gives little control over the infrastructure that your application runs on, which means that you are only responsible for creating the application itself and configuring its application settings, like connection strings to databases.

The highest level of abstraction is **software as a service (SaaS)**, which offers the least amount of control and the most amount of time that you can focus on working on business value. An example of Azure SaaS is [**Azure Cognitive Services**](https://azure.microsoft.com/services/cognitive-services/), which are APIs that you just call from your application. Somebody else owns their application code and infrastructure; you just consume them. And all you manage is basic application configuration, like managing the API keys that you use to call the service.

Once I know how much control I need, I can pick the category of services in Azure and narrow down my choice.

## Question 2: Where do I need my app to run?

The second question stemming from “Running my application in Azure” is: Where do I need my application to run?

![image info](/img/azure/12/Where-my-app-run.png)
*Figure 2. Where do I need my app to run?*

You might think that the answer would be: I need to run my application in Azure. But the answer may not be that simple. For example, maybe I do want parts of my application to run in the Azure public cloud but I want to run other parts in [Azure Government](https://azure.microsoft.com/global-infrastructure/government/) or the Azure China cloud or even on-premises using [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).

Or it could be that I want to be able run my application in Azure and on-premises (if rules and regulations change), on my local development computer, or even in public clouds from other vendors.

This question boils down to how vendor-agnostic I’d like to be and where to store my data.

Once I’ve answered this question, I can narrow down the choice of Azure services even further.

## Question 3: What usage model do I need?

How my app will be used guides me to the answer to the third and final question: what usage model do I need?

![image info](/img/azure/12/What-useage-model.png)
*Figure 3. What usage model do I need?*

Some applications **are in use all the time**, like a website. If that is the case for my application, I need to look for services that run on what I call the **classic model**. This means that they are always running and that you pay for them all month.

Other applications **are only in use occasionally**, like a background job that runs once every hour, or an API that handles order cancellations (called a few times a day). If my application runs occasionally, I need to select a service from the logic-as-a-service (or serverless) category. These services only run when you need them, and you only pay for them when they run.

After I’ve answered the high-level questions for this category, I can narrow down the services that I can choose from to just a couple or even one.

## Question 4: Which functionality do I need?

In this step: We match service functionality to my application requirements

Now, I need to figure out which service fulfills the most requirements for my application. I do so by looking at the functionality that I need, the availability that the service offers in its service level agreement, the performance that it provides, and what it costs.

Finally, this leads me to the service that I want to use to run my application. Often, I use multiple services to run my application, because it consists of many pieces, like a user interface and APIs. So, it might turn out that I run my web application in an Azure App Service Web App and my APIs in Azure Functions.

## Example : 

So, let's just try this out.I have an app, and **I trust Azure to scale for me**.I don't want to do that myself, **My app needs to run in Azure and on-premises as well**.**As my app only needs to run once every hour**, so not all the time.

My Need to run App.

* **I trust Azure to scale for me**
* **My app needs to run in Azure and on-premises as well**
* **My app only needs to run once every hour**

So let's see. These are all the options currently.They're out there in Azure to run my application.

![image info](/img/azure/12/Options_For_Running_App.png)
**Figure:Option for running Apps on Azure**

So, for example, I'm thinking,well, I could just put it into VM,it could be fine, but then I'm paying 24/7 all the time.

> But My app need are it doesn't have to be always on.

I have to remote it to desktop,I have to access or SSH into it and upgrade it.I have to maintain the security.

> But My app need are Azure to Scale it.

So, probably not a VM.

Exacty, So, services in the category into infrastructure as a service category,those are not the services that I want in this case.

Also services in the platform as a service category those also need scaling instructions, let's say.Azure doesn't do that automatically for me.Yes I can obviously have automatic scaling in a web app,but still I need to think about that and I need to configure it myself.And I don't want that for this app.

And even I could do Kubernetes and have that scale, but it will be running all the time. 

So, I end up in the last category and that is I call it logic Azure service or serverless,and that could be functions or logic apps for instance.
So that's one category that we've drilled down to. 

![image info](/img/azure/12/img2.png)

So, that ends up in me being able to work a lot on the business value,and not so much having to worry about the plumbing and infrastructure of a VM for instance.

So, the next thing is,**I want to Stay Vendor Agnostic**. That's what I said because **it needs to run in Azure but also on premises**. 

So, I don't want to opt in for Azure completely.it needs to run anywhere.In that case, I can choose from these services

![image info](/img/azure/12/img3.png)

including Azure functions because,Azure functions run on the Azure functions run-time.I can run that anywhere basically. 

I can take Azure functions as a docker container that I can build from and I could put that on anywhere, on a container locally On-premise Or on my own laptop.Wherever. So, that's still an options in this case.


Then the next one is I only want to run it occasionally.So not all the time.That's the serverless,those are Azure Batch,Logic apps and Azure functions.

![image info](/img/azure/12/img4.png)

So, I'm kind of drilling down to Azure functions in this case.For this particular app needs.

Then I will compare all the features of the Azure services to the requirements that I have.I'm not going to drill down into that right now,but I just wanted to show you that I do that in different tables like these.

And I don't just build a big table,where I just put in all of the Azure services,but just tables that compare one category to another.

For instance, these are all the services for container options and VMs,

<span style="color:yellow;background-color:black;"> **Below are the comparision Chart for quick reference**</span>

![image info](/img/azure/12/img5.png)

and these are all the services for background tasks, will be the one that I need.

![image info](/img/azure/12/img6.png)

And these are all the services for running your application in Azure.

![image info](/img/azure/12/img7.png)

So, that's my process for choosing how to run an application in Azure.And now the next part is,**how do I store my data in Azure?**

## Storing Data in Azure

![image info](/img/azure/12/storing_data.png)

 
> The process that I use for “Storing Data in Azure.”


How do I do that? Again the same thing.I have a couple of big picture questions that asks to narrow down to services.


![image info](/img/azure/12/storing_data_question.png)

* **What will I use the data for?**
* **What type of data am I going to store?**
* **Which functionality do I need?**

Once I’ve answered these questions, I’ve narrowed down the services from which to choose. And then, I look deeper into the services to see which one best matches the requirements of my application, including functionality as well as availability, performance, and costs.

## Question 1: What will I use the data for?

So, what will I use the data for? Is it for my application? 

So actually for my application,where I have a data stored on my website for instance that stores customer records or something,that's also called **Online Transactional Processing**.

The other usage that you can imagine is for data analytics, we do reporting.And that's called **Online Analytical Processing**.

![image info](/img/azure/12/What_will_I_use_data_for.png)

So that's the first divide that we do.

## Question 2: What type of data am I going to store?

There are lots of data types.One is Relational Data,another one is Unstructured Data.There are semi-structure data and all that stuff but we're just going to stick to
these for clarity and simplicity.

In unstructured Data you have different types: Document Data,key Value Data, and Graph Data as well.

![image info](/img/azure/12/What_type_of_data.png)


And then we're going to try it out,Here is my application data need

* **My app is an online reservation system**.

* **My app needs to store and retrieve document data.**

So that last sentence actually says it all.

For this example, we're going to focus on the store and retrieve document data in the application.

So, this is again an example of all of the services that are out there right now in Azure for storing data.

![image info](/img/azure/12/Options_for_storing_data.png)

These are things like Azure Sequel Database,Azure storage but also Azure sequel data warehouse.

So the first thing that we're going to do,is divide this into two categories of data usage.

![image info](/img/azure/12/data_category.png)

So Online Transactional Processing,
so the data storage that I actually
use for my application,
and Online Analytical Processing
data storage that I use for reporting.

So, in our case, it falls into the **first category**,because I want to store and retrieve data for my online reservation system that is online constantly.

And then the second one is, **what type of data**?

So again, these services are relational data,

![image info](/img/azure/12/Relational_data.png)

and these services I can store unstructured or document data and all the other sub types of that.

![image info](/img/azure/12/Unstructure_data.png)

and then these services as a Sequel data warehouse and data lakes store.
I can do data analytics,so I can store big chunks of data and use things like
Power BI or Hadoop Clusters actually crunched that data.

![image info](/img/azure/12/data_analytic.png)

And then after that, I would go into these tables,just like I did for the running the applications,and to see if the requirements match the functionality of the services.

![image info](/img/azure/12/data_compare.png)

And again, I compare services in
different categories like for OLAP and OLTP in this case.

![image info](/img/azure/12/data_compare1.png)

So that's my process basically.

So, these were just a couple
of services in the Azure space,
just for running your application and for storing data.

Obviously, there are lot of more services out there for securing your application and for doing IoT and all that stuff.