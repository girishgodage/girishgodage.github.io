---
title: How To Easily Create Azure Functions Using Azure Portal
date: 2020-10-07 11:41:00 Z
permalink: "/blog/how-to-easily-create-azure-functions-using-azure-portal"
categories:
- AzureCloud
tags:
- learning
summary: In this article, we are going to discuss Azure Functions and how you can easily create your first Azure function app using Azure Portal. This is the first article of my new series called Azure functions for beginners.
image: "/img/azureAD.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-10-07-how-to-easily-create-azure-functions-using-azure-portal
---

## Introduction

In this article, we are going to discuss Azure Functions and how you can easily create your first Azure function app using Azure Portal. This is the first article of my new series called Azure functions for beginners. So let's grab a cup of coffee and start learning.

## What are Azure Functions?
 
**Function As A Service (FaaS)** is a category of *cloud computing services* that provides a platform in which you just need to think about writing business logic without thinking about the infrastructure required for its execution. 

Microsoft Azure provides two types of FaaS i.e **Azure Logic Apps** and **Azure Functions**.

So in this article, we are going to discuss **Azure functions only.**

### According to **Microsoft docs**:
 
*Azure Functions allows you to run small pieces of code (called "functions") without worrying about application infrastructure. With Azure Functions, the cloud infrastructure provides all the up-to-date servers you need to keep your application running at scale.*

Azure functions are **event-driven** which means that you can run your function code when some event is triggered from either existing on-premise service or any other Azure service or third party service. The azure function can be scaled and you need to pay only for the resources as you consume. *Azure functions support lots of trigger points such as Http trigger, queue trigger, etc.*

## Some of the features of Azure functions are:

* Azure Functions has great language support such as C#, Java, JavaScript, Python, and PowerShell.
* Supports lots of triggers such as Service bus trigger, Http triggers, Queue trigger, etc.
* We need to pay only for the resources as you consume i.e. Pay-per-use pricing model
* We can install any of the libraries of your choice using NuGet or NPM
* We can easily implement CI/CD using Github, Azure DevOps Service
* We can also add integrated security on Http triggered based Azure functions using OAuth providers such as Azure Active Directory, Facebook, Google, Twitter, and Microsoft Account.

## Below are several templates available for creating Azure function for performing different operations:

* HTTP trigger
* Blob storage trigger
* Queue storage trigger
* Service Bus Queue trigger
* Service Bus Topic trigger
* Timer trigger 
* Event Grid trigger
* Event Hub trigger
* Azure Cosmos DB trigger

You can get more information about Azure Functions [here](https://docs.microsoft.com/en-us/azure/azure-functions/). So in this article, we are going to create an HTTP trigger-based Azure function using C# from Azure Portal. I will cover all other types of templates in my next article.

 
## Create Azure Function using Azure Portal 
 
### Prerequisites
* [Azure](https://portal.azure.com/) account (If you don't have an Azure subscription, [create a free trial account](https://azure.microsoft.com/free)) 
  
### Step 1 - Creating a Function App

While creating a function you must have a function app to host the execution of your functions. You can group multiple functions into one function app which helps for deployment, scaling, easy for development & sharing of resources One function app can have multiple functions. 
 
Log in to the [Azure portal](https://portal.azure.com/).

In the Search bar, search as "function app" and then select the Function app.

![image info](/img/azure/7/1.png)

![image info](/img/azure/7/2.png)

After that click on the **"Add"** button to add a new function app. Fill out the basic details:

![image info](/img/azure/7/3.png)

* Azure has Resource Groups(RG) which acts as a container for your resources. So now we are going to create a function app resource then first we need to create a Resource Group. If you have already created RG then you can use the same here. Under the Resource group click on the Create New button and give a unique RG name. I have Created a new resource group.
* You need to provide a unique name for your function app.
* Select an appropriate run time for your app. Since we are going to create a function using C# so I have used .net core as runtime.
* Select a version and region. 

Once we filled out basic details, then click on the **Next: Hosting button**.

![image info](/img/azure/7/4.png)

* You need to provide a storage account while creating a function app. If you have already created a Storage account then you can use the same here. Or you can create new as well.
* You have to select an operating system on which your app needs to be run.
* Finally, you have to select a pricing plan for your app. You can see more details about Pricing [here](https://azure.microsoft.com/pricing/details/functions/)
  
Once we filled out hosting details then click on the **Next: Monitoring button**.

![image info](/img/azure/7/5.png)

**Application Insights**, a feature of Azure Monitor, which is an extensible *Application Performance Management (APM) service* for developers and DevOps professionals. It is used to monitor your live applications. So if you want to integrate it in your function app the select Enable Application Insights flag to Yes.
 
Now click on **Review:Create button** and review all the details and click on the **Create button**. Wait for a few minutes to create the resources.

![image info](/img/azure/7/6.png)

Once deployment complete click on **Go to resource button** to see out new function app.

![image info](/img/azure/7/7.png)

![image info](/img/azure/7/8.png)

## Step 2 - Creating an HTTP triggered based Azure Function
 
Once you navigate to the Function app then click on Functions in the left panel and then click on the Add button.

![image info](/img/azure/7/9.png)

Now select the **HTTP trigger** from the right panel. Now set **Name and Authorization** level for your new function.

![image info](/img/azure/7/10.png)

* Provide a name for your new function.
* To restrict the use of your function we can set the Authorization level. There are three types of levels available: 
  * 1] When we set levels as Function, then we have to provide a function key to call our function.
  *  2] When we set as **Admin**, then you need to provide the master key. Both the function key and admin keys are found in the 'keys' management panel on the portal when your function is selected.
  *  3] When we don't want any authorization then we can simply set level as **Anonymous**.

Now click on the Create Function button. You then automatically redirect to function once the function is successfully created. 

![image info](/img/azure/7/11.png)

Click on the **"Code + Test" button** under the Developer section from the left sidebar panel. 

![image info](/img/azure/7/12.png)

So you can see the basic function code written which accepts name from the query string or from the request body and append to name to the Hello string and return the output.
 
## Step 3 - Testing of Http triggered based Azure Function
 
Now click on the "Test/Run" button. For this default function, you can provide the input property name from either body or either from the query string.

![image info](/img/azure/7/13.png)

Click on the **Run button** from the right panel. So you can see the output in the **"Output"** tab in the right panel.

![image info](/img/azure/7/14.png)

This is all about running it from Portal. You can run this function from the browser or from the postman as well. To get the function URL click on the **"Get Function URL"** button.

![image info](/img/azure/7/15.png)

Copy the URL.

![image info](/img/azure/7/16.png)

Since our function accepts input property name from the query string then paste the URL into the browser and append `?name=AzFunction` and hit the URL. You will see the output in the browser.

![image info](/img/azure/7/17.png)

Since this is the HTTP Trigger function we can run it from postman as well. So paste the URL in postman and pass input and see the output.

![image info](/img/azure/7/18.png)

That's it. You have created your Azure function using Azure Portal.
 
## Conclusion
 
In this article, I have explained Azure Functions and how to create a function using the Azure Portal. Also, I demonstrated how to test function from portal, browser, and postman. I really hope that you enjoyed this article, share it with friends, and please do not hesitate to send me your thoughts or comments. Stay tuned for more Azure Functions articles.

