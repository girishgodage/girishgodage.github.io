---
title: The service desk with bots using Amazon Lex and Amazon Connect (Part 2)
date: 2020-02-02 11:41:00 Z
permalink: "/blog/create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-2"
categories:
- AWSCloud
tags:
- learning
summary: What! Is this guy for real? Does he really think he can replace the front line of IT with pre-canned conversations?" I must admit, it's a bold statement. The IT Service Desk has been around for years and has been the foot in the door for most of us in the IT industry. It's the face of IT operations and plays an important role in ensuring an organisation's staff can perform to the best of their abilities. But what if we could take some of the repetitive tasks the service desk performs and automate them? Not only would we be saving on head count costs, we would be able to ensure correct policies and procedures are followed to uphold security and compliance. The aim of this blog is to provide a working example of the automation of one service desk scenario to show how easy and accessible the technology is, and how it can be applied to various use cases.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: '/blog/create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-1'
nexturl: '/blog/create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-2'
discussion_id: 2020-02-02-create-service-desk-with-bots-using-amazon-lex-and-amazon-connect-part-2
---

## The service desk with bots using Amazon Lex and Amazon Connect (Part 2)


Hopefully you've had the chance to follow along in parts 1  where we set up our Lex chatbot to take and validate input.

In this blog, we'll interface with our Active Directory environment to perform the password reset function. To do this, we need to create a Lambda function that will be used as the logic to fulfil the user's intent. The Lambda function will be packaged with the python LDAP library to modify the AD password attribute for the user. Below are the components that need to be configured.

## Active Directory Service Account

To begin, we need to start by creating a service account in Active Directory that has permissions to modify the password attribute for all users. Our Lambda function will then use this service account to perform password resets. To do this, create a service account in your Active Directory domain and perform the following to delegate the required permissions:

  1. Open Active Directory Users and Computers.
  2. Right click the OU or container that contains organisational users and click **Delegate Control**
  3. Click **Next** on the Welcome Wizard.
  4. Click **Add** and enter the service account that will be granted the reset password permission.
  5. Click **OK** once you've made your selection, followed by **Next**.
  6. Ensure that **Delegate the following common tasks** is enabled, and select **Reset user passwords and force password change at next logon**.
  7. Click **Next**, and **Finish**


![image info](/img/awscloud/19/delegatecontrol.png)

## KMS Key for the AD Service Account

Our Lambda function will need to use the credentials for the service account to perform password resets. We want to avoid storing credentials within our Lambda function, so we'll store the password as an encrypted Lambda variable and allow our Lambda function to decrypt it using Amazon's Key Management Service (KMS). To create the KMS encryption key, perform the following steps:

  1. In the Amazon Console, navigate to **IAM**
  2. Select **Encryption Keys**, then Create key
  3. Provide an Alias (e.g. resetpw) then select **Next**
  4. Select **Next Step** for all subsequent steps then **Finish** on step 5 to create the key.

## IAM Role for the Lambda Function

Because our Lambda function will need access to several AWS services such as SNS to notify the user of their new password and KMS to decrypt the service account password, we need to provide our function with an IAM role that has the relevant permissions. To do this, perform the following steps:


1. In the Amazon Console, navigate to **IAM**
2. Select **Policies** then **Create policy**
3. Switch to the **JSON** editor, then copy and paste the following policy, replacing the KMS resource with the KMS ARN created above
   ```
    {
    "Version": "2012-10-17",
    "Statement": [
        {
        "Effect": "Allow",
        "Action": "sns:Publish",
        "Resource": "*"
        },
        {
        "Effect": "Allow",
        "Action": "kms:Decrypt",
        "Resource": "<KMS ARN>"
        }
    ]
    }
    ```
4. Provide a name for the policy (e.g. resetpw), then select **Create policy**
5. After the policy has been created, select **Roles**, then **Create Role**
6. For the AWS Service, select **Lambda**, then click **Next:Permissions**
7. Search for and select the policy you created in step 5, as well as the **AWSLambdaVPCAccessExecutionRole** and **AWSLambdaBasicExecutionRole** policies then click **Next: Review**
8. Provide a name for the role (e.g. resetpw) then click **Create role**

## Network Configuration for the Lambda Function

To access our Active Directory environment, the Lambda function will need to run within a VPC or a peered VPC that hosts an Active Directory domain controller. Additionally, we need the function to access the internet to be able to access the KMS and SNS services. Lambda functions provisioned in a VPC are assigned an Elastic network Interface with a private IP address from the subnet it's deployed in. Because the ENI doesn't have a public IP address, it cannot simply leverage an internet gateway attached to the VPC for internet connectivity and as a result, must have traffic routed through a NAT Gateway similar to the diagram below.

![image info](/img/awscloud/19/natgw.png)





{% include blogslide.html %}

