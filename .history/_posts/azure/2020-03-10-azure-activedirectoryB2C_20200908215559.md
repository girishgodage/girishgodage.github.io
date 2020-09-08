---
title: Azure Active Directory B2C
date: 2020-03-10 10:41:00 Z
permalink: "/blog/azure-acticedirectoryB2C"
categories:
- AzureCloud
tags:
- learning
summary: Azure Active Directory B2C (Azure AD B2C) is an identity management service that enables custom control of how your customers sign up, sign in, and manage their profiles when using your iOS, Android, .NET, single-page (SPA), and other applications.In this Article we learn overview and technical features
image: "/img/azureAD.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-azure-acticedirectoryB2C
---

>## What is Azure Active Directory B2C?

 **Azure Active Directory B2C** provides business-to-customer **identity as a service**. Your customers use their preferred social, enterprise, or local account identities to get single sign-on access to your applications and APIs.

![image info](/img/azure/3/azureadb2c-overview.png)
 

**Azure Active Directory B2C (Azure AD B2C)** is a customer identity access management (CIAM) solution capable of **supporting millions of users and billions of authentications per day**. 
It takes care of the **scaling** and **safety** of the *authentication platform*, monitoring and automatically handling threats like **denial-of-service, password spray, or brute force attacks**.

## Custom-branded identity solution

 Azure AD B2C is a **white-label authentication solution**. You can customize the entire user experience with your brand so that it blends seamlessly with your web and mobile applications.

**Customize every page displayed by Azure AD B2C when your users sign up, sign in, and modify their profile information**. Customize the **HTML, CSS, and JavaScript** in your user journeys so that the Azure AD B2C experience **looks and feels like it's a native part of your application**. 

![image info](/img/azure/3/sign-in-small.png)

## Single sign-on access with a user-provided identity

Azure AD B2C uses standards-based authentication protocols including **OpenID Connect, OAuth 2.0, and SAML**. It integrates with most modern applications and commercial off-the-shelf software.

![image info](/img/azure/3/scenario-singlesignon.png)

By serving as the **central authentication authority** for your ***web applications, mobile apps, and API***s, Azure AD B2C enables you to build a single **sign-on (SSO) solution** for them all. Centralize the collection of **user profile and preference information**, and *capture detailed analytics* about **sign-in behavior** and **sign-up conversion**.

## Integrate with external user stores

Azure AD B2C provides a directory that **can hold 100 custom attributes per user**. However, you can also **integrate with external systems**. For example, use Azure AD B2C for **authentication**, but delegate to an external **customer relationship management (CRM) ** or **customer loyalty database** as the source of truth for customer data.

Another external user store scenario is to have Azure AD B2C handle the authentication for your application, but **integrate with an external system that stores user profile or personal data**. For example, to satisfy data residency requirements like **regional or on-premises data** storage policies.

![image info](/img/azure/3/scenario-remoteprofile.png)

Azure AD B2C can facilitate **collecting the information from the user during registration or profile editing**, then *hand that data off to the external system*. Then, during future **authentications**, Azure AD B2C can retrieve the data from the external system and, if needed, include it as a part of the authentication token response it sends to your application.

## Progressive profiling

Another user journey option includes **progressive profiling**. Progressive profiling allows your **customers to quickly complete their first transaction by collecting a minimal amount of information**. Then, gradually collect more profile data from the customer on future sign-ins.

![image info](/img/azure/3/scenario-progressive.png)

## Third-party identity verification and proofing

Use Azure AD B2C to facilitate *identity verification and proofing by collecting user data, then passing it to a third party system to perform validation, trust scoring, and approval for user account creation*.

![image info](/img/azure/3/scenario-idproofing.png)

These are just some of the things you can do with Azure AD B2C as your business-to-customer identity platform.

> ## Technical and feature overview of Azure Active Directory B2C

Here are the **primary resources** you work with in the service, its features, and how these enable you to provide a fully custom identity experience for your customers in your applications.

## Azure AD B2C tenant

In Azure Active Directory B2C (Azure AD B2C), **a tenant** represents **your organization** and is a **directory of users**. *Each Azure AD B2C tenant is distinct and separate from other Azure AD B2C tenants*. An Azure AD B2C tenant is different than an Azure Active Directory tenant, which you may already have.

The primary resources you work with in an Azure AD B2C tenant are:

* **Directory** - The directory is where Azure AD B2C **stores** your **users' credentials** and **profile data**, as well as your **application registrations**.
  
* **Application registrations** - You **register your web, mobile, and native applications** with Azure AD B2C to **enable identity management**. Also, **any APIs** you want to protect with Azure AD B2C.
* **User flows and custom policies** - The built-in **(user flows)** and fully customizable **(custom policies)** identity experiences for your applications.
    * Use user flows for **quick configuration and enablement of common identity tasks like sign up, sign in, and profile editing**.
    * Use **custom policies** to enable user experiences not only for the **common identity tasks**, but also for crafting support for **complex identity workflows** unique to your organization, customers, employees, partners, and citizens.
* **Identity providers** - Federation settings for:
    * **Social identity** providers like Facebook, LinkedIn, or Twitter that you want to support in your applications.
    * **External identity** providers that support standard identity protocols like OAuth 2.0, OpenID Connect, and more.
    * **Local accounts** that enable users to sign up and sign in with a username (or email address or other ID) and password.
* **Keys** - Add and manage encryption keys for signing and validating tokens, client secrets, certificates, and passwords.
  
An **Azure AD B2C tenant** is the **first resource** you need to create to get started with Azure AD B2C. Learn how in Tutorial: [**Create an Azure Active Directory B2C tenant**]().

## Accounts in Azure AD B2C
Azure AD B2C defines **several types of user accounts**. Azure Active Directory, Azure Active Directory **B2B**, and Azure Active Directory **B2C** share these account types.

* **Work account** - Users with work accounts can manage resources in a tenant, and with an **administrator role**, can also manage tenants. Users with work accounts can *create new consumer accounts, reset passwords, block/unblock accounts, and set permissions or assign an account to a security group.*
* **Guest account** - External users you invite to your tenant as guests. A typical scenario for inviting a guest user to your Azure AD B2C tenant **is to share administration responsibilities**.
* **Consumer account** - Consumer accounts are the accounts created in your Azure AD B2C directory when **users complete the sign-up user journey** in an application you've registered in your tenant.

![image info](/img/azure/3/portal-01-users.png)

## Consumer accounts
With a consumer account, users can sign in to the applications that you've secured with Azure AD B2C. Users with consumer accounts can't, however, access Azure resources, for example the Azure portal.

A consumer account can be associated with these identity types:

* **Local identity**, with the username and password stored locally in the Azure AD B2C directory. We often refer to these identities as **"local accounts."**
  
* **Social or enterprise identities**, where the identity of the user is managed by a federated identity provider like Facebook, Microsoft, ADFS, or Salesforce.
  
A user with a consumer account can sign in with multiple identities, for example username, email, employee ID, government ID, and others. A single account can have multiple identities, both local and social.

![image info](/img/azure/3/identities.png)

Azure AD B2C lets you manage common attributes of consumer account profiles like **display name, surname, given name, city, and others**. You can also **extend the Azure AD schema** to store additional information about your users. For example, their **country/region** or **residency**, **preferred language**, and **preferences** like whether they want to **subscribe to a newsletter** or **enable multi-factor authentication**.

## External identity providers

You can configure Azure AD B2C to allow users to sign in to your application with credentials from **external social or enterprise identity providers (IdP)**. Azure AD B2C supports external identity providers like **Facebook, Microsoft account, Google, Twitter, and any identity provider** that supports *OAuth 1.0, OAuth 2.0, OpenID Connect, and SAML* protocols.

![image info](/img/azure/3/external-idps.png)

With external identity provider federation, you can offer your consumers the ability to **sign in with their existing social or enterprise accounts**, *without having to create a new account just for your application.*

On the sign-up or sign-in page, Azure AD B2C presents a list of external identity providers the user can choose for **sign-in**. Once they select one of the external identity providers, they're taken (redirected) to the selected provider's website to complete the sign in process. After the user successfully signs in, they're returned to Azure AD B2C for authentication of the account in your application.

![image info](/img/azure/3/external-idp.png)

## Identity experiences: user flows or custom policies

The extensible policy framework of Azure AD B2C is its core strength. Policies describe your users' identity experiences such as sign up, sign in, and profile editing.

In Azure AD B2C, there are **two primary paths** you can take to provide these identity experiences: **user flows** and **custom policies**.

* **User flows** are predefined, built-in, configurable policies that we provide so you can create **sign-up, sign-in, and policy editing** experiences in minutes.

* **Custom policies** enable you to create your own user journeys for complex identity experience scenarios.

Both user flows and custom policies are powered by the **Identity Experience Framework, Azure AD B2C's policy orchestration engine.**

## User flow
To help you quickly set up the most common identity tasks, the Azure portal includes several predefined and configurable policies called user flows.

You can configure user flow settings like these to control identity experience behaviors in your applications:

  * Account types used for sign-in, such as social accounts like a Facebook, or local accounts that use an email address and password for sign-in
  * Attributes to be collected from the consumer, such as first name, postal code, or country/region of residency
  * Azure Multi-Factor Authentication (MFA)
  * Customization of the user interface
  * Set of claims in a token that your application receives after the user completes the user flow
  * Session management
  * ...and more.
  
Most common identity scenarios for the majority of mobile, web, and single-page applications can be defined and implemented effectively with user flows. We recommend that you use the built-in user flows unless you have complex user journey scenarios that require the full flexibility of custom policies.

## Custom policy
Custom policies unlock access to the full power of the **Identity Experience Framework (IEF) orchestration engine**. With custom policies, you can leverage IEF to build almost any authentication, user registration, or profile editing experience that you can imagine.

The Identity Experience Framework gives you the ability to construct user journeys with any combination of steps. For example:

* Federate with other identity providers
* First- and third-party multi-factor authentication (MFA) challenges
* Collect any user input
* Integrate with external systems using REST API communication

Each such user journey is defined by a policy, and you can build as many or as few policies as you need to enable the best user experience for your organization.

![image info](/img/azure/3/custom-policy.png)

A custom policy is defined by several XML files that refer to each other in a hierarchical chain. The XML elements define the claims schema, claims transformations, content definitions, claims providers, technical profiles, user journey orchestration steps, and other aspects of the identity experience.

The powerful flexibility of custom policies is most appropriate for when you need to build complex identity scenarios. Developers configuring custom policies must define the trusted relationships in careful detail to include metadata endpoints, exact claims exchange definitions, and configure secrets, keys, and certificates as needed by each identity provider.

## Protocols and tokens

* For applications, Azure AD B2C supports the **OAuth 2.0, OpenID Connect, and SAML protocols** for user journeys. Your application starts the user journey by issuing authentication requests to Azure AD B2C. The result of a request to Azure AD B2C is a **security token**, such as an **ID token, access token, or SAML token**. This security token defines the user's identity within the application.

* For external identities, Azure AD B2C supports **federation with any OAuth 1.0, OAuth 2.0, OpenID Connect, and SAML identity providers.**

The following diagram shows how Azure AD B2C can communicate using a variety of protocols within the same authentication flow:

![image info](/img/azure/3/protocols.png)

1. The relying party application initiates an authorization request to Azure AD B2C using OpenID Connect.
2. When a user of the application chooses to sign in using an external identity provider that uses the SAML protocol, Azure AD B2C invokes the SAML protocol to communicate with that identity provider.
3. After the user completes the sign-in operation with the external identity provider, Azure AD B2C then returns the token to the relying party application using OpenID Connect.

## Application integration
When a user wants to sign in to your application, whether it's a web, mobile, desktop, or single-page application (SPA), the application initiates an authorization request to a user flow- or custom policy-provided endpoint. The user flow or custom policy defines and controls the user's experience. When they complete a user flow, for example the sign-up or sign-in flow, Azure AD B2C generates a token, then redirects the user back to your application

![image info](/img/azure/3/app-integration.png)

Multiple applications can use the same user flow or custom policy. A single application can use multiple user flows or custom policies.

**For example**, to sign in to an application, the application uses the sign up or sign in user flow. After the user has signed in, they may want to edit their profile, so the application initiates another authorization request, this time using the profile edit user flow.

## Seamless user experiences
In Azure AD B2C, you can craft your users' identity experiences so that the pages they're shown blend seamlessly with the look and feel of your brand. You get nearly full control of the HTML and CSS content presented to your users when they proceed through your application's identity journeys. With this flexibility, you can maintain brand and visual consistency between your application and Azure AD B2C.

![image info](/img/azure/3/seamless-ux.png)

## Localization
Language customization in Azure AD B2C allows you to accommodate different languages to suit your customer needs. **Microsoft provides the translations for 36 languages**, but you can also provide your own translations for any language. Even if your experience is provided for only a single language, you can customize any text on the pages.

![image info](/img/azure/3/localization.png)

## Add your own business logic

If you choose to use custom policies, you can integrate with a RESTful API in a user journey to add your own business logic to the journey. For example, Azure AD B2C can exchange data with a RESTful service to:

* Display custom user-friendly error messages.
* Validate user input to prevent malformed data from persisting in your user directory. For example, you can modify the data entered by the user, such as capitalizing their first name if they entered it in all lowercase.
* Enrich user data by further integrating with your corporate line-of-business application.
* Using RESTful calls, you can send push notifications, update corporate databases, run a user migration process, manage permissions, audit databases, and more.
  
Loyalty programs are another scenario enabled by Azure AD B2C's support for calling REST APIs. For example, your RESTful service can receive a user's email address, query your customer database, then return the user's loyalty number to Azure AD B2C. The return data can be stored in the user's directory account in Azure AD B2C, then be further evaluated in subsequent steps in the policy, or be included in the access token.

![image info](/img/azure/3/lob-integration.png)

You can add a REST API call at any step in the user journey defined by a custom policy. For example, you can call a REST API:

* During sign-in, just before Azure AD B2C validates the credentials
* Immediately after sign-in
* Before Azure AD B2C creates a new account in the directory
* After Azure AD B2C creates a new account in the directory
* Before Azure AD B2C issues an access token

## Protect customer identities
Azure AD B2C complies with the security, privacy, and other commitments described in the Microsoft Azure Trust Center.

Sessions are modeled as encrypted data, with the decryption key known only to the Azure AD B2C Security Token Service. A strong encryption algorithm, AES-192, is used. All communication paths are protected with TLS for confidentiality and integrity. Our Security Token Service uses an Extended Validation (EV) certificate for TLS. In general, the Security Token Service mitigates cross-site scripting (XSS) attacks by not rendering untrusted input.

![image info](/img/azure/3/user-data.png)

## Access to user data

Azure AD B2C tenants share many characteristics with enterprise Azure Active Directory tenants used for employees and partners. Shared aspects include mechanisms for viewing administrative roles, assigning roles, and auditing activities.

You can assign roles to control who can perform certain administrative actions in Azure AD B2C, including:

* Create and manage all aspects of user flows
* Create and manage the attribute schema available to all user flows
* Configure identity providers for use in direct federation
* Create and manage trust framework policies in the Identity Experience Framework (custom policies)
* Manage secrets for federation and encryption in the Identity Experience Framework (custom policies)
  

## Multi-factor authentication (MFA)

Azure AD B2C multi-factor authentication (MFA) helps safeguard access to data and applications while maintaining simplicity for your users. It provides additional security by requiring a second form of authentication, and delivers strong authentication by offering a range of easy-to-use authentication methods. Your users may or may not be challenged for MFA based on configuration decisions that you can make as an administrator.

## Smart account lockout

To prevent brute-force password guessing attempts, Azure AD B2C uses a sophisticated strategy to lock accounts based on the IP of the request, the passwords entered, and several other factors. The duration of the lockout is automatically increased based on risk and the number of attempts.

![image info](/img/azure/3/smart-lockout1.png)

## Password complexity

During sign up or password reset, your users must supply a password that meets complexity rules. By default, Azure AD B2C enforces a strong password policy. Azure AD B2C also provides configuration options for specifying the complexity requirements of the passwords your customers use.

You can configure password complexity requirements in both **user flows** and **custom policies**.

## Auditing and logs

Azure AD B2C emits audit logs containing activity information about its resources, issued tokens, and administrator access. You can use these audit logs to understand platform activity and diagnose issues. Audit log entries are available soon after the activity that generated the event occurs.

In an audit log, which is available for your Azure AD B2C tenant or for a particular user, you can find information including:

* Activities concerning the authorization of a user to access B2C resources (for example, an administrator accessing a list of B2C policies)
* Activities related to directory attributes retrieved when an administrator signs in using the Azure portal
* Create, read, update, and delete (CRUD) operations on B2C applications
* CRUD operations on keys stored in a B2C key container
* CRUD operations on B2C resources (for example, policies and identity providers)
* Validation of user credentials and token issuance

![image info](/img/azure/3/audit-log.png)

## Usage insights

Azure AD B2C allows you to discover when people sign up or sign in to your web app, where your users are located, and what browsers and operating systems they use. By integrating Azure Application Insights into Azure AD B2C by using custom policies, you can gain insight into how people sign up, sign in, reset their password or edit their profile. With such knowledge, you can make data-driven decisions for your upcoming development cycles.

