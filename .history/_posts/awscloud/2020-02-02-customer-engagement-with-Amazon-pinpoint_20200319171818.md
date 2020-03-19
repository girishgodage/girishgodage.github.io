---
title: Amazon pinpoint - Customer Engagement
date: 2020-02-02 11:41:00 Z
permalink: "/blog/customer-engagement-with-Amazon-pinpoint"
categories:
- AWSCloud
tags:
- learning
summary: Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels. You can use Amazon Pinpoint to send push notifications, emails, SMS text messages, and voice messages.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-02-02-customer-engagement-with-Amazon-pinpoint

---

## What is Customer Engagement?

*Customer engagement* refers to the practices that organizations employ to keep users coming back to their applications. Maximizing customer engagement is vital in today's digital marketplace, where an ever-increasing number of brands are competing for a finite amount of customer attention. In this article, we discuss the factors that impact customer engagement, and provide information about specific steps you can take to improve customer engagement.

### Why customer engagement matters

In 2012, the average adult in the United States spent one hour per day using his or her mobile device; by 2017, that figure had increased to 5.6 hours per day. Over half of that time is spent using third-party apps, but the majority of app users access 20 or fewer apps each month. Of those 20 apps, most users spent the vast majority of their time in their top 10 applications.

At the same time, first impressions are everything in today's digital marketplace. Almost a quarter of users have downloaded an app, ran it once, and never opened it again. Additional research suggests that as many as 80% of all app users stop using a given app within 90 days.

These statistics paint a very clear picture: in order to succeed in this highly competitive environment, organizations must deliver digital content that delights and engages their customers.

### Customer engagement in action
Let's assume your organization produces a mobile app for sports fans called All Things Sports. In your app, users can get the latest sports news, purchase memorabilia and event tickets, and play exciting mini-games. Your organization has devoted a large amount of time, effort, and money into creating a best-in-class app and acquiring new users. For this reason, it's essential that your users keep coming back to your application so that your business can grow.

For all applications, user interactions fall into three categories: activation, retention, and conversion. All Things Sports requires users to confirm their email address and select their favorite sports teams; the percentage of users who complete this process is the activation rate. Next, you need to offer content that is compelling enough to keep your users coming back. All Things Sports offers fun mini-games and special offers that encourage users to return to the app often. The likelihood that your customers will return to your application repeatedly is referred to as your retention rate. Finally, your organization needs to make money in order to grow. The main revenue generating activity in All Things Sports is the sale of event tickets and merchandise. The percentage of users who purchase these products through your app is your conversion rate. As the application developer, you have to continuously improve all three of these metrics in order to grow your business.

The first step toward improving these metrics is to gather customer analytics data. Customer analytics data is the data you collect and analyze to better understand the ways in which customers use your application. A thorough analysis of this data can help you better serve your users, thereby increasing their engagement with your application. Making optimizations to your application based on this data can lead to positive reviews, better app store rankings, and increased word of mouth. These factors can increase your activation rate, which can in turn increase your retention and conversion rates. You can start gathering this information by integrating the AWS Mobile SDK into your application. For more information about customer analytics, see our companion article, What are Customer Analytics?

The next step toward improving customer engagement is to create targeted segments of customers based on their characteristics. By targeting specific customers based on their demographics, preferences, behaviors, and interests, you can ensure that you're using the right channels to send the right messages to the right customers. **Amazon Pinpoint** can import app usage data obtained from the AWS Mobile SDK and use it to create customer segments. To learn more about the ways in which you can create customer segments, see our companion article, What is Customer Segmentation?

The final step toward improving customer engagement is to deploy personalized, multi-channel messaging to your customers. You can use these messages to proactively update engaged customers on exciting new features and improvements in your application, or to incentivize less-engaged customers to sign back in to your application. You can also use this messaging to convey vital transactional content—such as password reset emails or purchase confirmations—to all customers. You can also use **Amazon Pinpoint** to meet this need. For more information about multi-channel messaging, see the companion article titled What is Multi-Channel Messaging?

## What Is Amazon Pinpoint?

Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels. You can use **Amazon Pinpoint to send push notifications, emails, SMS text messages, and voice messages**.

## Amazon Pinpoint Features
This section describes the major features of Amazon Pinpoint and the tasks that you can perform by using them.

### Define Audience Segments
Reach the right audience for your messages by defining audience segments. A segment designates which users receive the messages that are sent from a campaign or journey. You can define dynamic segments based on data that's reported by your application, such as operating system or mobile device type. You can also import static segments that you define outside of Amazon Pinpoint.

### Engage Your Audience with Messaging Campaigns
Engage your audience by creating a messaging campaign. A campaign sends tailored messages on a schedule that you define. You can create campaigns that send push notifications, email, SMS text messages, and voice messages.

To experiment with alternative campaign strategies, set up your campaign as an A/B test, and analyze the results with Amazon Pinpoint analytics.

### Create User Journeys
Create custom, multi-step experiences for your customers by designing and building journeys. With journeys, you can send messages to your customers based on their attributes, behaviors, and activities. When you build a journey, you design an automated workflow of activities that perform a variety of different actions—for example, sending an email message to participants, waiting for a certain period of time, or splitting participants based on actions that they take, such as clicking a link in a message.

### Provide Consistent Messaging with Templates
Design consistent messages and reuse content more effectively by creating and using message templates. A message template contains content and settings that you want to reuse in messages that you send for any of your Amazon Pinpoint projects. You can use message templates in email messages, push notifications, SMS messages, and voice messages.

### Deliver Personalized Content
Send content that's customized for each recipient of a message. Using message variables and attributes, you can deliver dynamic, personalized content in messages that you send from campaigns and journeys.

To streamline development, you can also use message variables and attributes to add personalized content to message templates. With message templates, this content can come from attributes that you create directly in Amazon Pinpoint or a machine learning model that you create in Amazon Personalize. By connecting message templates to models in Amazon Personalize, you can use machine learning to send relevant promotions or recommendations to each recipient of a message.

### Analyze User Behavior
Gain insight into your audience and the effectiveness of your campaigns and messaging activities by using the analytics that Amazon Pinpoint provides. You can view trends in your users' level of engagement, purchase activity, demographics, and more. You can also monitor your message traffic by viewing metrics such as the total number of messages that you sent for a campaign or project. Through the Amazon Pinpoint API, your application can also report custom data, which Amazon Pinpoint makes available for analysis.

To analyze or store analytics data outside Amazon Pinpoint, configure Amazon Pinpoint to stream the data to Amazon Kinesis.

### Send Test Messages
Test the design and deliverability of your messages by sending test messages before you send messages to your customers.

## Get Started
Get started with Amazon Pinpoint by **creating a new project** or **completing a tutorial**.

## Getting Started with Amazon Pinpoint

To start sending targeted messages in Amazon Pinpoint, you have to complete a few steps. *For example, you have to add customer contact information into Amazon Pinpoint, and then create segments that target certain customers. Next, you have to create your messages and schedule your campaigns. Finally, after you send your campaigns, you can use the analytics dashboards that are built into Amazon Pinpoint to see how well the campaigns performed.*

### Intended Audience

This tutorial is designed for **marketing and business users**.

### Features Used

This tutorial shows you how to complete all of the following steps by using the Amazon Pinpoint console:

*   Importing customer data from a file.

*   Creating a segment that targets specific users based on their attributes.

*   Creating an email campaign and scheduling it to be sent at a specific time.

*   Viewing email delivery and response data by using the analytics dashboards that are built into Amazon Pinpoint.


### Step 1: Create and Configure a Project

To create a project and verify an email address

1. Open the Amazon Pinpoint console at https://console.aws.amazon.com/pinpoint/.

2. On the **All projects page**, choose **Create a project**.

3. On the **Create a project** window, for **Project name**, enter a name for your project, and then **choose Create**.

**Note**
*The project name can contain up to 64 characters.*

4. On the **Configure features** page, next to **Email**, choose **Configure**.

5. For Email address, type an email address that you want to use to send email. For example, you can use your personal email address, or your work email address. Choose **Verify**.

6. Wait for 1–2 minutes, and then check the inbox for the email address that you specified in step 4. You should see an email from Amazon Web Services (no-reply-aws@amazon.com) with the subject line "Amazon Web Services – Email Address Verification Request in region **RegionName**", where **RegionName** is the name of the AWS Region that you're configuring Amazon Pinpoint in.

7. Open the email, and then click the link in the body of the email.

8. Return to the Amazon Pinpoint console in your browser. On the Set up email page, choose Save.

Your account is now ready to send email from the email address that you verified. You can add additional email addresses later.

You can also verify entire domains. When you verify a domain, you can send email from any address on that domain. For more information, see Verifying a Domain.

### Step 2: Import Customer Data and Create a Segment

A segment is a group of your customers that share certain attributes. *For example, a segment might contain all of your customers who use version 2.0 of your app on an Android device, or all customers who live in the city of Los Angeles.*

When you create a campaign, you have to choose a segment to send the campaign to. You can send multiple campaigns to a single segment, and you can send a single campaign to multiple segments.

There are two types of segments that you can create in Amazon Pinpoint:

* **Dynamic segments** – Segments that are based on attributes that you define. Dynamic segments can change over time. For example, if you add new endpoints to Amazon Pinpoint, or if you modify or delete existing endpoints, the number of endpoints in that segment may increase or decrease.

* **Imported segments** – Segments that are created outside of Amazon Pinpoint and saved in CSV or JSON format. Imported segments are static—that is, they never change. When you create a new segment, you can use an imported segment as a base segment, and then refine it by adding filters.

In this tutorial, you create an imported segment by uploading a file from your computer. Next, you create a dynamic segment that is based upon the imported segment.

#### Step 2.1: Download and Modify the Sample File

In this section, you download a file that contains fictitious customer data. You also modify the data to include your own contact information. Later in this tutorial, you use this data to create a segment.


1. In a web browser, download the sample file from https://raw.githubusercontent.com/awsdocs/amazon-pinpoint-user-guide/master/examples/Pinpoint_Sample_Import.csv. Save the file to your computer.

  ***Tip**
    You can quickly save this file to your computer by right-clicking the link, and then choosing Save Link As.*

2. Open the file in a text editor or spreadsheet application. On the last row of the file, replace the items in angle brackets (<…>) with your own contact information.

    In the Address column, provide the same email address that you verified in Step 1.

    In the User.UserAttributes.Company column, specify a company name that's different from the fictitious company names in the file. You'll use this unique company name when you define the criteria for your targeted segment in the next section.

    **Note**
    *You don't have to provide your information for each column in the file. However, at a minimum, you have to provide information for the ChannelType, Address, and User.UserAttributes.Company columns.*

    The email that you create later in this tutorial uses several of these fields to create a personalized message.

3. When you finish, save the file.

    **Note**
    *If you used a spreadsheet application to modify the file, make sure that you save the modified file in Comma-Separated Values (.csv) format. Amazon Pinpoint can only import .csv and .json files.*

#### Step 2.2: Import a File that Contains Customer Data

Now that you have a file that contains customer data, you can import it into Amazon Pinpoint. To import customer data, you have to create a new segment.

**To create an imported segment**

1. In the Amazon Pinpoint console, in the navigation pane, choose **Segments**.

2. Choose **Create a segment**.

3. On the **Create a segment** page, choose **Import a segment**.

4. In the **Specifications** section, under **Import method**, choose **Upload files from your computer**.

5. Select **Choose files**. Navigate to the Pinpoint_Sample_Import.csv file that you downloaded and modified in the previous section.

6. Choose **Create segment**. Amazon Pinpoint copies the file from your computer and creates a segment. Wait for about 1 minute while the import completes.

#### Step 2.3: Create a Targeted Segment
Your Amazon Pinpoint project now contains some customer data, as well as a segment that contains your entire customer list. It also contains your contact information.

In this section, you create a targeted segment. You add segment criteria that filter the segment so that you're the only member of the segment.

**To create the segment**

1. On the **Segments** page, choose **Create a segment**.

2. On the **Create a segment** page, choose **Build a segment**.

3. For **Name**, enter a name for the segment.

4.Under **Segment group 1**, do the following:

* Next to **Include endpoints** that are in any of the following segments, choose the **Pinpoint_Sample_Import** segment that you created in the previous step.
* Under **Add filters to refine your segment**, from the menu, choose **Filter by channel**.

* Next to **Endpoints that match**, choose **all**.

* For **Channel**, choose **EMAIL**.

* Under **Add filters to refine your segment**, from the menu, choose **Filter by user**.

* In the **User** filter, use the menu to choose **Company**. Next, use the **Choose values** menu to choose the unique company name that you specified for your own contact record in step 2.1.
  
* Choose **Add an attribute or metric**.

* In the new filter, use the menu to choose **First Name**. Next, use the **Choose values** menu to choose your **first name**.

* Choose **Create segment**.

### Step 3: Create and Schedule a Campaign

A campaign is a messaging initiative that engages a specific audience segment. A campaign sends tailored messages on the days and times that you specify. You can use the console to create a campaign that sends messages through the email, push notification, or SMS channels.

In this section, you create an email campaign. You create a new campaign, choose your target segment, and create a responsive email message for the campaign. When you finish setting up the message, you choose the day and time when you want the message to be sent.

#### Step 3.1: Create the Campaign and Choose a Segment
When you create a segment, you first give the segment a name. Next, you choose the segment that the campaign applies to. In this tutorial, you choose the segment that you created in Step 2.3.

To create the campaign and choose segment

1. In a web browser, download the sample file from https://raw.githubusercontent.com/awsdocs/amazon-pinpoint-user-guide/master/examples/Pinpoint_Sample_Email.html. Save the file to your computer.

    Tip
    You can quickly save this file to your computer by right-clicking the link, and then choosing Save Link As.

2. Open the file that you just downloaded in a text editor, such as Notepad (Windows) or TextEdit (macOS). Press Ctrl+A (Windows) or Cmd+A (macOS) to select all of the text. Then, press Ctrl+C (Windows) or Cmd+C (macOS) to copy it.

3. In the Amazon Pinpoint console, in the navigation pane, choose Campaigns.

4. Choose Create a campaign.

5. Under Campaign details, for Campaign name, enter a name for the campaign.

6. For Campaign type, choose Standard campaign.

7. For Choose a channel for this campaign, choose Email.

8. Choose Next.

9. On the Choose a segment page, choose Use an existing segment. Then, for Segment, choose the targeted segment that you created in Step 2.3. Choose Next.

#### Step 3.2: Create the Campaign Message
After you specify a campaign name and choose a segment, you can create your message. This tutorial includes a link to an HTML file that you can use to create your message.

This sample file uses responsive HTML to create a message that renders properly on both computers and mobile devices. It uses inline CSS to provide compatibility with a wide variety of email clients. It also includes tags that are used to personalize the message with the recipient's name and other personal information.

To create the message

1. On the Create your message page, under Message content, choose Create a new message.

2. For Subject, enter a subject line for the email.

3. Under Message, erase the sample HTML code that's shown in the editor. Paste the HTML code that you copied in the first step in this section.

4. (Optional) Modify the content of the message to include a message that you want to send.

You can personalize the message for each recipient by including the name of an attribute inside two sets of curly braces. For example, the sample message includes the following text: {{User.UserAttributes.FirstName}}. This code represents the User.UserAttributes.FirstName attribute, which contains the recipient's first name. When you send the campaign, Amazon Pinpoint removes this attribute name and replaces it with the appropriate value for each recipient.

You can experiment with other attribute names. Refer to the column headers in the spreadsheet that you imported in Step 2.2 for complete list of attribute names that you can specify in your message.

Tip
You can use Design view to edit the content of the message without having to edit the HTML code. To use this view, choose Design from the view selector above the message editor, as shown in the following image.


Choose Next.

##### Step 3.3: Schedule the Campaign
The last step in creating the campaign is to choose when to send it. In Amazon Pinpoint, you can set up your campaigns so that they're sent immediately after you launch them. You can also schedule them to be sent in the future—anywhere from 15 minutes from the current time, to six months into the future. Finally, you can schedule your messages to be sent on a recurring basis (that is, hourly, daily, weekly, or monthly). Recurring campaigns are a great way to send account or status updates where the appearance of the campaign message stays the same over time, but is populated with information that changes dynamically.

In this section, you schedule your campaign to be sent immediately after you launch it.

To schedule the campaign

On the Choose when to send the campaign page, choose At a specific time. Then, under Choose when the campaign should be sent, choose Immediately. Finally, choose Next.

On the Review and launch page, review all of the details of the campaign. When you're ready to send it, choose Launch campaign.

Congratulations—you've created your first campaign with Amazon Pinpoint! Because you're the only member of the segment that you created in Step 2.3, you should receive the message in your inbox within a few seconds.

### Step 4: View Campaign Analytics

At this point, you've created a segment that you're a member of. You've also created an email campaign and sent it to yourself. In this section, you look at the delivery and response metrics for the campaign.

#### Step 4.1: Interact with Your Campaign
Before you can view the delivery and response metrics for your campaign, you have to interact with the message that you sent yourself in Step 3.

To interact with the email

In your email client, open the message that you sent yourself in Step 3.

If your email client automatically hides images by default, choose the Download pictures (or equivalent) button to load the images in the message.

Choose one or more of the links that are contained in the message.

Wait for a few minutes, and then proceed to the next section.

#### Step 4.2: View Metrics for the Campaign
After you interact with the email that you sent from the campaign, you can view the metrics for the campaign.

To view the campaign metrics

Open the Amazon Pinpoint console at https://console.aws.amazon.com/pinpoint/.

On the All projects page, choose the project that you used to send the campaign.

In the navigation pane, under Analytics, choose Campaigns.

In the Campaigns section, choose the campaign that you created in Step 3.

(Optional) Use the date control to choose a date range for the reports on this page.

On the metrics page for your campaign, you see the following information:

Delivery count metrics – This section provides information about the delivery of the messages that were sent from your campaign. It includes the following information:

Messages sent – The number of messages that were sent.

Messages delivered – The number of messages that were delivered to their recipients.

Links clicked – The number of times that links in the messages were clicked by recipients. If a single recipient clicks a link more than once, each click is represented in this section.

Endpoint deliveries – The average number of endpoints that the campaign was sent to, for each day in the chosen date range. The chart shows the number of endpoints that the campaign was delivered to, for each day in the chosen date range.

Delivery rate metrics – This section shows the overall delivery and response rates for the messages that were sent from your campaign. It includes the following information:

Delivery rate – The percentage of messages that were delivered to recipients, of the total number of endpoints that you targeted in the segment that you sent this campaign to.

Email open rate – The percentage of messages that were opened by recipients, of the total number of messages that were delivered.

Bounce rate – The percentage of messages that weren't delivered to recipients because they bounced. This value includes only hard bounces—that is, messages that bounced because of a permanent issue. For example, hard bounces could occur when the recipient's email address doesn't exist, or when the recipient permanently rejects email from your domain.

Campaign runs – This section shows information that's specific to each time the campaign ran. Because you can use Amazon Pinpoint to create recurring campaigns, this section can show information for several campaign runs. However, if you completed the procedures in this tutorial, this section contains information for only one campaign run because you ran the campaign only once. This section contains the following metrics, in addition to the metrics that are defined in the preceding sections:

Endpoints targeted – The number of endpoints that were targeted by the segment that was associated with the campaign run. This number includes endpoints that were part of the segment, but didn't receive the message.

Total email opened – The total number of times that messages sent from the campaign run were opened. For example, if a message was opened two times by one recipient, both of those opens are counted.

## Next Step

We hope that you use this tutorial as a starting point as you discover the additional capabilities of Amazon Pinpoint. For example:

* You can improve the delivery of your email campaigns by making sure that your campaigns align with industry best practices. For more information, see Tips and Best Practices.

* You can verify an entire domain, which allows you to send email from any address on that domain. For more information about verifying domains, see Verifying a Domain.

* You can obtain dedicated IP addresses for sending your email. Dedicated IP addresses are a great option for sending email in certain use cases. For more information, see Using Dedicated IP Addresses with Amazon Pinpoint.

* You can enable the Amazon Pinpoint Deliverability dashboard. The Deliverability dashboard helps you identify issues that could impact the delivery of your emails. For more information, see The Amazon Pinpoint Deliverability Dashboard.

* You can send messages through other channels, such as SMS or push. Before you can use these channels, you have to enable and configure them on the Settings page. For more information about using the Settings page to enable and configure channels, see Amazon Pinpoint Settings.

* You can send data about your campaigns outside of Amazon Pinpoint. For example, you can send delivery and response data for your campaigns to Amazon S3 for long-term storage. You can also send data to Amazon Redshift to perform custom analyses. For more information about sending your data outside of Amazon Pinpoint, see Event Stream Settings.

* You can integrate Amazon Pinpoint with your apps, or interact with Amazon Pinpoint programmatically, by using an AWS SDK. For more information, see the Amazon Pinpoint Developer Guide.