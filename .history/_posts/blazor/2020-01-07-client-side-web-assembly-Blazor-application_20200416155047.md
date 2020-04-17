---
title: Creating A Step-By-Step End-To-End Database Client-Side (WebAssembly) Blazor Application
date: 2020-01-07 10:41:00 Z
permalink: "/blog/Client-Side-Web-Assembly-Blazor-Application"
categories:
- Blazor
tags:
- learning
summary: In this article, using client (WebAssembly) Blazor, we will demonstrate how a list of Weather forecasts can be added to the database by each user. A user will only have the ability to see their own forecasts.
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-Client-Side-Web-Assembly-Blazor-Application
---

### Creating A Step-By-Step End-To-End Database Client-Side (WebAssembly) Blazor Application

 In this article, using client (WebAssembly) Blazor, we will demonstrate how a list of Weather forecasts can be added to the database by each user. A user will only have the ability to see their own forecasts.

 ![image info](/img/blazor/1/BlazorClientADEndToEnd.gif)

##  Walk-Thru

![image info](/img/blazor/1/Creating-A-Step-By-Step-End-To-End-Database-image.png)

When we navigate to the **Fetch data** page, without being logged in, it will indicate that you **Must be logged** in.

To log in, click the **Log In** link.

![image info](/img/blazor/1/Creating-A-Step-By-Step-End-To-End-Datab_58BF-image.png)

You can log in with an **Azure Active Directory** account that has [authorization to log into the Azure registered application](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/developer-guidance-for-integrating-applications).

![image info](/img/blazor/1/3.png)

Once a user is *logged in*, navigating to the **Fetch data** page displays their data.

The **logged in user's name** is also displayed at the top of the application.

The user can *add* or *edit* **Weather Forecasts** that will be saved to the database.

 
## Starting With The Blazor Client AD Code

![image info](/img/blazor/1/BlazorClientADAnimation.gif)

You can easily implement **authentication** for your **Client Side Blazor applications** using **Azure Active Directory**.

We do this by [Implementing a custom AuthenticationStateProvider](https://docs.microsoft.com/en-us/aspnet/core/security/blazor/?view=aspnetcore-3.1&tabs=visual-studio#implement-a-custom-authenticationstateprovider).

## Walk-Thru

![image info](/img/blazor/1/5.png)

When we navigate to the **Fetch data** page, without being *logged in*, it will indicate that you **Must be logged in**.

To *log in*, click the **Log In link**.

![image info](/img/blazor/1/6.png)

You can log in with an **Azure Active Directory** account that has [authorization to log into the Azure registered application](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/developer-guidance-for-integrating-applications).








 