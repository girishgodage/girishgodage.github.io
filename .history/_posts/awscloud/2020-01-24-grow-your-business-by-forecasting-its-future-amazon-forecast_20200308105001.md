---
title: Grow your business by forecasting its future Amazon Forecast
date: 2020-01-24 10:41:00 Z
permalink: "/blog/grow-your-business-by-forecasting-its-future-amazon-forecast"
categories:
- AWSCloud
tags:
- learning
summary: Amazon Forecast is a fully managed service that uses machine learning to
  deliver highly accurate forecasts. It can achieve forecasting accuracy levels that
  used to take months of engineering in as little as a few hours with no machine learning
  experience to get started.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-24-AmazonForecast
---

## Amazon Forecast -  Grow your business by forecasting its future

---

Want to predict your upcoming sales revenue??

Want to know appropriate inventory levels of a particular item to stock??

Want to predict the right level of available resources, such as staffing levels, raw material etc. to maximize revenue and control costs??

---

To run a successful business, you should be ready for the future. No matter the scale of the business, you should be ready for any circumstance. Almost all companies have been facing the problem of predicting its sales revenue, inventory management, workforce and resource management, financial planning, supply chain, healthcare, etc.

Before machine learning, people could only predict based on their experiences. But obviously, that cannot be precise. Since all the companies have been storing all these transactions (sales figures, inventory data) for a long time, this can be used to forecast the future using machine learning.

> A good forecaster is not smarter than everyone else. He merely has his ignorance better organized.

## Amazon Forecast:

Amazon Forecast is a fully managed service that uses machine learning to deliver highly accurate forecasts. It can achieve forecasting accuracy levels that used to take months of engineering in as little as a few hours with no machine learning experience to get started.

### Steps:

* **Create Dataset and Import data**
* **Train a Predictor**
* **Generate Forecasts**
  
### Problem Statement ([Download Dataset](https://drive.google.com/open?id=1vel5vEv12hW5QM8SMiThbFPKoAbpNL7F)):

Predicting the sales of an item for the next 10 days based on past sales records for 30 months.

### Create Dataset and Import data:

To Start Forecasting your data, Upload it to an S3 Bucket.

Open **Amazon Forecast console** and select "Create Dataset Group".

To create a dataset group, there are 3 more steps:
    * Create Dataset Group
    * Create Target Time Series Dataset
    * Import Target Time Series Data

### Create Dataset Group:

You can give any Dataset Group Name and since our dataset is of forecasting retail we have selected Domain as "Retail".