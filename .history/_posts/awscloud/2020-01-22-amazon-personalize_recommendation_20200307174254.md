---
title: Amazon Personalize can now use 10X more item attributes to improve relevance
  of recommendations
date: 2020-01-22 10:41:00 Z
permalink: "/blog/amazon-personalize-can-now-use-10x-more-item-attributes-to-improve-relevance-of-recommendations"
categories:
- AWSCloud
tags:
- learning
summary: Amazon Personalize is a machine learning service which enables you to personalize
  your website, app, ads, emails, and more, with custom machine learning models which
  can be created in Amazon Personalize, with no prior machine learning experience.
  AWS is pleased to announce that Amazon Personalize now supports ten times more item
  attributes for modeling in Personalize. Previously, you could use up to five item
  attributes while building an ML model in Amazon Personalize. This limit is now 50
  attributes. You can now use more information about your items, for example, category,
  brand, price, duration, size, author, year of release etc., to increase the relevance
  of recommendations..
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-22-AmzonPersonalize_reccomender
---

## Amazon Personalize - can now use 10X more item attributes to improve relevance of recommendations

**Amazon Personalize** is a machine learning service which enables you to **personalize your website, app, ads, emails, and more,** with custom machine learning models which can be created in Amazon Personalize, with no prior machine learning experience. AWS is pleased to announce that Amazon Personalize now supports ten times more item attributes for modeling in Personalize. Previously, you could use up to five item attributes while building an ML model in Amazon Personalize. This limit is now 50 attributes. You can now use more information about your items, ***for example, category, brand, price, duration, size, author, year of release etc., to increase the relevance of recommendations.***

In this post, you learn how to add item metadata with custom attributes to Amazon Personalize and create a model using this data and user interactions. This post uses the Amazon customer reviews data for beauty products. For more information and to download this data, see **Amazon Customer Reviews Dataset**. We will use the history of what items the users have reviewed along with user and item metadata to generate product recommendations for them.

## Pre-processing the data

To model the data in Amazon Personalize, you need to break it into the following datasets:

* **Users** – Contains metadata about the users
* **Items** – Contains metadata about the items
* **Interactions** – Contains interactions (for this post, reviews) and metadata about the interactions

For each respective dataset, this post uses the following attributes:

* **Users** – customer_id, helpful_votes, and total_votes
* **Items** – product_id, product_category, and product_parent
* **Interactions** – product_id, customer_id, review_date, and star_rating

This post does not use the other attributes available, which include *marketplace, review_id, product_title, vine, verified_purchase, review_headline, and review_body.*

Additionally, to conform with the keywords in **Amazon Personalize**, this post **renames customer_id to USER_ID, product_id to ITEM_ID, and review_date to TIMESTAMP.**

To download and process the data for input to Amazon Personalize, use the following Python example codes.

For the Users dataset, enter the following code:

```
Bash
#Downloading data
$aws s3 cp s3://amazon-reviews-pds/tsv/amazon_reviews_us_Beauty_v1_00.tsv.gz .
$gunzip amazon_reviews_us_Beauty_v1_00.tsv.gz 
```

```python
#Generating the user dataset
import pandas as pd
fields = ['customer_id', 'helpful_votes', 'total_votes']
df = pd.read_csv('amazon_reviews_us_Beauty_v1_00.tsv', sep='\t', usecols=fields)
df = df.rename(columns={'customer_id':'USER_ID'})
df.to_csv('User_dataset.csv', index = None, header=True)

```