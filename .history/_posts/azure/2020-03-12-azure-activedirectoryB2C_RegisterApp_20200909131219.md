---
title: Register a web application in Azure Active Directory B2C
date: 2020-03-12 12:41:00 Z
permalink: "/blog/azure-acticedirectoryB2C_RegisterApp"
categories:
- AzureCloud
tags:
- learning
summary: In this article, you learn how to Register a web application,Create a client secret
image: "/img/azureAD.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-03-12-azure-acticedirectoryB2C_RegisterApp
---

>## Register a web application in Azure Active Directory B2C

Before your applications can interact with Azure Active Directory B2C (Azure AD B2C), they must be registered in a tenant that you manage. This tutorial shows you how to register a web application using the Azure portal.

In this article, you learn how to:

* Register a web application
* Create a client secret
  
If you're using a native app instead (e.g. iOS, Android, mobile & desktop), learn how to register a native client application.

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Prerequisites

If you haven't already created your own [Azure AD B2C Tenant](azure-activedirectoryB2C_CreateTenant), create one now. You can use an existing Azure AD B2C tenant.

## Register a web application

To register an application in your Azure AD B2C tenant, you can use new unified App registrations experience 

1. Sign in to the [Azure portal](https://portal.azure.com/).

2. Select the **Directory + Subscription** icon in the portal toolbar, and then select the directory that contains your Azure AD B2C tenant.

3. In the Azure portal, search for and select **Azure AD B2C**.

4. Select **App registrations**, and then select **New registration**.

5. Enter a **Name** for the application. For example, webapp1.

6. Under **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**

7. Under **Redirect URI**, select **Web**, and then enter https://jwt.ms in the URL text box.

The redirect URI is the endpoint to which the user is sent by the authorization server (Azure AD B2C, in this case) after completing its interaction with the user, and to which an access token or authorization code is sent upon successful authorization. In a production application, it's typically a publicly accessible endpoint where your app is running, like https://contoso.com/auth-response. For testing purposes like this tutorial, you can set it to https://jwt.ms, a Microsoft-owned web application that displays the decoded contents of a token (the contents of the token never leave your browser). During app development, **you might add the endpoint where your application listens locally, like https://localhost:5000**. You can add and modify redirect URIs in your registered applications at any time.

The following restrictions apply to redirect URIs:

   * The reply URL must begin with the scheme https.
   * The reply URL is case-sensitive. Its case must match the case of the URL path of your running application. For example, if your application includes as part of its path .../abc/response-oidc, do not specify .../ABC/response-oidc in the reply URL. Because the web browser treats paths as case-sensitive, cookies associated with .../abc/response-oidc may be excluded if redirected to the case-mismatched .../ABC/response-oidc URL.
  
  
8. Under Permissions, select the Grant admin consent to openid and offline_access permissions check box.

9. Select Register.

Once the application registration is complete, enable the implicit grant flow:

## Create a client secret

If your application exchanges an authorization code for an access token, you need to create an application secret.

1. In the **Azure AD B2C - App registrations** page, select the application you created, for example webapp1.
2. In the left menu, under **Manage**, select **Certificates & secrets**.
3. Select **New client secret**.
4. Enter a description for the client secret in the **Description** box. For example, clientsecret1.
5. Under **Expires**, select a duration for which the secret is valid, and then select **Add**.
6. Record the secret's **Value**. You use this value as the application secret in your application's code.

## Next steps
In this article, you learned how to:

* Register a web application
* Create a client secret
  
Next, learn how to [create user flows](azure-acticedirectoryB2C_CreateUserFlow) to enable your users to sign up, sign in, and manage their profiles.

