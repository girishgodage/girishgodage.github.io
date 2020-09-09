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
Under Permissions, select the Grant admin consent to openid and offline_access permissions check box.

Select Register.

Once the application registration is complete, enable the implicit grant flow:

## Create an Azure AD B2C tenant

1. Sign in to the [Azure portal](https://portal.azure.com/). Sign in with an Azure account that's been assigned at least the [Contributor](https://docs.microsoft.com/en-in/azure/role-based-access-control/built-in-roles) role within the subscription or a resource group within the subscription.

2. Select the directory that contains your subscription.

In the Azure portal toolbar, select the Directory + Subscription icon, and then select the directory that contains your subscription. This directory is different from the one that will contain your Azure AD B2C tenant.

![image info](/img/azure/4/portal-01-pick-directory.png)

3. On the Azure portal menu or from the **Home** page, select **Create a resource**.

4. Search for **Azure Active Directory B2C**, and then select **Create**.

5. Select **Create a new Azure AD B2C Tenant**.

![image info](/img/azure/4/portal-02-create-tenant.png)

6. On the **Create a directory** page, enter the following:

* **Organization name** - Enter a name for your Azure AD B2C tenant.
* **Initial domain name** - Enter a domain name for your Azure AD B2C tenant.
* **Country or region** - Select your country or region from the list. This selection can't be changed later.
* **Subscription** - Select your subscription from the list.
  * **Resource group** - Select a resource group that will contain the tenant. Or select **Create new**, enter a **Name** for the resource group, select the **Resource group location**, and then select **OK**. 

![image info](/img/azure/4/review-and-create-tenant.png)

7. Select **Review + create**.

8. Review your directory settings. Then select **Create**.

You can link multiple Azure AD B2C tenants to a single Azure subscription for billing purposes. To link a tenant, you must be an admin in the Azure AD B2C tenant and be assigned at least a Contributor role within the Azure subscription.

## Select your B2C tenant directory

To start using your new Azure AD B2C tenant, you need to switch to the directory that contains the tenant.

Select the **Directory + subscription** filter in the top menu of the Azure portal, then select the directory that contains your Azure AD B2C tenant.

If at first you don't see your new Azure B2C tenant in the list, refresh your browser window, then select the **Directory + subscription** filter again in the top menu.

![image info](/img/azure/4/portal-07-select-tenant-directory.png)

## Add Azure AD B2C as a favorite (optional)

This optional step makes it easier to select your Azure AD B2C tenant in the following and all subsequent tutorials.

Instead of searching for Azure AD B2C in **All services** every time you want to work with your tenant, you can instead favorite the resource. Then, you can select it from the portal menu's **Favorites** section to quickly browse to your Azure AD B2C tenant.

You only need to perform this operation once. Before performing these steps, make sure you've switched to the directory containing your Azure AD B2C tenant as described in the previous section, [Select your B2C tenant directory](azure-acticedirectoryB2C_CreateTenant#select-your-b2c-tenant-directory).

1. Sign in to the [Azure portal](https://portal.azure.com/).

2. In the Azure portal menu, select **All services**.

3. In the **All services** search box, search for Azure AD B2C, hover over the search result, and then select the star icon in the tooltip. Azure AD B2C now appears in the Azure portal under Favorites.

4. If you want to change the position of your new favorite, go to the Azure portal menu, select Azure AD B2C, and then drag it up or down to the desired position.

![image info](/img/azure/4/portal-08-b2c-favorite.png)

## Next steps

In this article, you learned how to:

* Create an Azure AD B2C tenant
* Link your tenant to your subscription
* Switch to the directory containing your Azure AD B2C tenant
* Add the Azure AD B2C resource as a Favorite in the Azure portal
* 
Next, learn [how to register a web application]() in your new tenant.
