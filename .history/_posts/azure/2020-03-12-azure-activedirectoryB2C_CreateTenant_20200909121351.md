---
title: Create an Azure AD B2C tenant
date: 2020-03-12 10:41:00 Z
permalink: "/blog/azure-acticedirectoryB2C_CreateTenant"
categories:
- AzureCloud
tags:
- learning
summary: In this article, you learn how to Create an Azure AD B2C tenant,Link your tenant to your subscription,Switch to the directory containing your Azure AD B2C tenant,Add the Azure AD B2C resource as a Favorite in the Azure portal
image: "/img/azureAD.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-03-12-azure-acticedirectoryB2C_CreateTenant
---

>## Create an Azure AD B2C tenant

Before your applications can interact with Azure Active Directory B2C (Azure AD B2C), **they must be registered in a tenant that you manage**.

In this article, you learn how to:

* Create an Azure AD B2C tenant
* Link your tenant to your subscription
* Switch to the directory containing your Azure AD B2C tenant
* Add the Azure AD B2C resource as a Favorite in the Azure portal

## Prerequisites

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

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

