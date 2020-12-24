---
title: Facebook Authentication And Authorization In Server-Side Blazor App
date: 2020-12-20 09:46:00 Z
permalink: "/blog/facebook-authentication-and-authorization-in-server-side-blazor-app"
categories:
- Blazor
- Azure
tags:
- learning
summary: In this article, we will learn how to implement authentication and authorization using Facebook in a server-side Blazor application.
image: "/img/blazor/5/output.gif"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-02-20-facebook-authentication-and-authorization-in-server-side-blazor-app
---

## Introduction

In this article, we will learn how to implement authentication and authorization using Facebook in a server-side Blazor application.

## Prerequisites
  * Install the latest .NET Core 5.0 Current  SDK from [here](https://dotnet.microsoft.com/download/dotnet-core/).
  * Install the latest Visual Studio 2019 from [here](https://visualstudio.com/).
  
## Source Code

Get the source code from [GitHub](https://github.com/girishgodage/Facebook-Authentication-with-server-side-Blazor).

## Create Server Side Blazor Application

To create a server-side Blazor app, open Visual Studio 2019 and follow the steps mentioned below:

    1. Click on “Create a new project”.
    2. Select “Blazor App” from available project types. Click on Next.
    3. A new “Configure your new project” screen will open. Put the name of the project as BlazorFbAuth and click Create.
    4. In the next screen, select “.NET.5.0 ” from dropdowns on the top left.
    5. Select “Blazor Server App” from the list of available templates.
    6. Click on Change Authentication button, a “Change Authentication” dialog box will open.
    7.Select “Individual User Account” and click OK. Click on Create button to create the application.

These steps are shown in the GIF image below.

![image info](/img/blazor/5/CreateProject.gif)


Before running the application, we need to apply migrations to our app. Navigate to Tools >> NuGet Package Manager >> Package Manager Console.

It will open the Package Manager Console. Put in **Update-Database** command and hit enter. This will update the database using Entity Framework Code First Migrations.

![image info](/img/blazor/5/UpdateDatabase.png)

Right-click on the project in solution explorer and select properties. Select Debug from left side menu then scroll to the bottom of the page. Note the SSL enabled URL. In this case, the URL is https://localhost:44345/. We need this URL to configure the Facebook app which we will be doing in our next section. Refer to the image below.

![image info](/img/blazor/5/AppURL.png)

## Creating a Facebook App

We need to create a Facebook app and configure Facebook Login for it. We will then use the App ID and App Secret of this Facebook app to implement Facebook authentication in our Blazor app. Navigate to https://developers.facebook.com/apps/ and sign in using your Facebook account. Follow the steps mentioned below.

 * Click on the “Create App” button under the “My Apps” menu on the top.
 * It will open a “Create a New App ID” dialog box. Put in the “Display Name” and “Contact Email”.
 * The default email will be your Facebook email id. However, you can change it and put any email of your choice. Click on “Create App ID” to create the app. Refer to the image below.

![image info](/img/blazor/5/BlazorFBAuthAppInfo.png)

 * Navigate to the app dashboard by clicking on the “Dashboard” link on the navigation menu on the left.
 * Under the “Add a Product” section, select the “Facebook Login” product and click on “Set Up” button.
 * A QuickStart wizard will be launched asking you to select a platform for the app. Skip this wizard and click on Facebook Login > Settings from the navigation menu on the left.
 * You will be navigated to the Client OAuth Settings page. In the Valid OAuth redirect URIs field enter the base URL of your application with /signin-facebook appended to it. For this tutorial, the URL will be https://localhost:44345/signin-facebook. Click on “Save Changes” button.
 * Now click on Settings > Basic on the navigation menu. You will see the App ID and App Secret values. Make a note of these values as we need them to configure Facebook authentication in our Blazor app.
  

Refer to the GIF below for a better understanding.

![image info](/img/blazor/5/FBSettings.gif)


## Important Note

> The trademark or brand element of Facebook is not allowed to be used as the Display Name of your Facebook App. Therefore, words such as FB, Face, Book, Insta, etc. cannot be used as Display Name.


## Installing Facebook authentication middleware NuGet package

To configure the ASP.NET Core middleware for Facebook authentication we need to install the nuget package in our application. The version of this nuget package must match the version of .NET Core 3 which we are using in our project.

Open https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook/. Select the version of .NET 5.0.1 from the “Version History”. Copy the command from the “package manager” tab. Run this command in the NuGet package manager console of our application.

For this application, we are using .NET 5.0.1. Therefore, we will run the following command in the package manager console of our application.

```code
    Install-Package Microsoft.AspNetCore.Authentication.Facebook -Version 5.0.1
```

Refer to the image below:

![image info](/img/blazor/5/InstallFBAuthentication.png)

## Configure the server-side Blazor app to use Facebook authentication

We need to store the App ID and App Secret field values in our application. We will use Secret Manager tool for this purpose. The Secret Manager tool is a project tool that is used to store secrets such as password, API Key, etc. for a .NET Core project during the development process. Secret Manager tool allows us to associate app secrets with a specific project. It also allow us to share them across multiple projects.

Open our web application once again and Right-click the project in Solution Explorer. Select Manage User Secrets from the context menu. A secrets.json file will open. Put the following code in it.

```code
    {
        "Authentication:Facebook:AppId": "Your Facebook AppId",
        "Authentication:Facebook:AppSecret": "Your Facebook AppSecret"
    }
```

Now open Startup.cs file and put the following code into ConfigureServices method.

```code

    services.AddAuthentication().AddFacebook(facebookOptions =>
    {
        facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
        facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
        facebookOptions.Events = new OAuthEvents()
        {
            OnRemoteFailure = loginFailureHandler =>
            {
            var authProperties = facebookOptions.StateDataFormat.Unprotect(loginFailureHandler.Request.Query["state"]);
            loginFailureHandler.Response.Redirect("/Identity/Account/Login");
            loginFailureHandler.HandleResponse();
            return Task.FromResult(0);
            }
        };
    });
```

This code will read the AppId and AppSecret from the secrets.json file. The AddFacebook() method is an extension method and it is used to configure the Facebook Authentication options for our application. We are also handling the event of OnRemoteFailure in this code section. Hence, if the user denies access to his Facebook account, then he will be redirected back to the Login page of our Blazor application.

## Adding authorization to Blazor pages

Blazor has added a new built-in component called AuthorizeView. This component is used to display different content based on the authentication state of the application. This component will display the child component only when the user is authorized.  The AuthorizeView component is configured in \Shared\LoginDisplay.razor file.

To implement authorization to a specific page, we need to use the [Authorize] attribute. Blazor has introduced a new directive @attribute, which is used to include the [Authorize] attribute for a page. In this application, we will apply [Authorize] to the FetchData component. This will prohibit unauthorized access to this component. Open FetchData.razor page and add the following lines at the top of the page.

```code

@attribute [Authorize]

```

## Execution Demo

Launch the application. Navigate to Fetch Data component by clicking on the “Fetch data” link on the menu on the left. You will see a “Not authorized” message displayed on the screen. Click “Log In” on the menu at the top. In the next page click on the “Facebook” button to login with Facebook. On the next page, you will be asked to provide the login credentials of your Facebook account. Fill the details and click Log In. Upon successful login, you will be able to access the Fetch Data component. If you do not want to login then click on “Not now”. It will redirect you back to the Login page of Blazor app.

Refer to the GIF below for a better understanding.

![image info](/img/blazor/5/output.gif)

Once you are logged in successfully into our Blazor app using Facebook, you will be also logged in to https://www.facebook.com/. This will create a set of browser cookie for https://www.facebook.com/. Therefore, the Blazor app will not ask the Facebook credentials when you try to login again. If you log out from Facebook then you have to enter credentials while logging into Blazor app.

## Conclusion

We learned how to implement Facebook authentication and authorization in a server-side Blazor application. We have created and configured a Facebook app to implement Facebook authentication. To implement authorization for a specific component in Blazor, we have used the [Authorize] attribute. We have used Microsoft.AspNetCore.Authentication.Facebook nuget package to configure the middleware for Facebook authentication.

Please get the source code from [GitHub](https://github.com/girishgodage/Facebook-Authentication-with-server-side-Blazor) and play around to get a better understanding.