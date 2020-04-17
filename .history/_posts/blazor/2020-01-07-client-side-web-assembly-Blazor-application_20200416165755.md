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


![image info](/img/blazor/1/7.png)

Once the user is *logged in*, navigating to the **Fetch data** page displays the data.

The **logged in user's name** is *also displayed* at the top of the application.

## Step 1 : Install Blazor Client Side (Blazor WebAssembly)

See the following directions to install Blazor WebAssembly (Client Side Blazor):

https://docs.microsoft.com/en-us/aspnet/core/blazor/get-started?view=aspnetcore-3.1&tabs=visual-studio

 

## Create The Project

Open **Visual Studio 2019 (or higher)**.

![image info](/img/blazor/1/8.png)

Create a **new project**.

![image info](/img/blazor/1/9.png)

Select **Blazor App**.

![image info](/img/blazor/1/10.png)

Name the project **BlazorClientAD**

![image info](/img/blazor/1/11.png)

Select the **ASP.NET 3.1 (or higher)** Blazor WebAssembly template and select **ASP.NET Core hosted**.

 ![image info](/img/blazor/1/12.png)

 The **Solution** will be created.

Open the **launchSettings.json** file.

![image info](/img/blazor/1/13.png)

Note the **sslPort**.

You will need it in the next section.

## Set-Up The Application In Azure

![image info](/img/blazor/1/14.png)

You will need a [free Azure Account](https://azure.microsoft.com/en-us/free) for the following steps.

Log into https://portal.azure.com/ and select the **Azure Active Directory** node.

![image info](/img/blazor/1/15.png)

Next, select **App registrations**.

![image info](/img/blazor/1/16.png)

Select **New registration**.

![image info](/img/blazor/1/17.png)

Create a Registration.

For Redirect URI, select Web, and use the following url (replacing {your sslPort} with the number you copied earlier):

> https://localhost:{your sslPort}/signin-oidc

![image info](/img/blazor/1/18.png)

On the Overview node for the application, copy the **Application (client) ID**.

![image info](/img/blazor/1/19.png)

Click on the **Authentication** node and **enable ID Tokens**.

## Configure The Application

![image info](/img/blazor/1/20.png)

Return to **Visual Studio**, and add a **appsettings.json** file to the **Server** project using the following code (replacing **{Your Application (client ID)}** with the ID you copied in the previous step):

```dotnetcli

{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/common",
    "ClientId": "{Your Application (client) ID}",
    "CallbackPath": "/signin-oidc"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```