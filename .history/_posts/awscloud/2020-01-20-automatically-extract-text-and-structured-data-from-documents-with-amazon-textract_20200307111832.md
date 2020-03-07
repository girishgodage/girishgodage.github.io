---
title: Automatically extract text and structured data from documents with Amazon Textract
date: 2020-01-20 10:41:00 Z
permalink: "/blog/automatically-extract-text-and-structured-data-from-documents-with-amazon-textract"
categories:
- AWSCloud
tags:
- learning
summary: Documents are a primary tool for record keeping, communication, collaboration, and transactions across many industries, including financial, medical, legal, and real estate. The millions of mortgage applications and hundreds of millions of W2 tax forms processed each year are just a few examples of such documents. A lot of information is locked in unstructured documents.
It usually requires time-consuming and complex processes to enable search and discovery, business process automation, and compliance control for these documents.
In this post, I show how you can take advantage of Amazon Textract to automatically extract text and data from scanned documents without any machine learning (ML) experience.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-20-AmzonTextract
---

## Amazon Textract - Automatically extract text and structured data from documents

Documents are a primary tool for record keeping, communication, collaboration, and transactions across many industries, including financial, medical, legal, and real estate. The millions of mortgage applications and hundreds of millions of W2 tax forms processed each year are just a few examples of such documents. A lot of information is locked in unstructured documents. It usually requires time-consuming and complex processes to enable search and discovery, business process automation, and compliance control for these documents.

In this post, I show how you can take advantage of Amazon Textract to automatically extract text and data from scanned documents without any machine learning (ML) experience. While AWS takes care of building, training, and deploying advanced ML models in a highly available and scalable environment, you take advantage of these models with simple-to-use API actions. Here are the use cases that I cover in this post:

* Text detection from documents
* Multi-column detection and reading order
* Natural language processing and document classification
* Natural language processing for medical documents
* Document translation
* Search and discovery
* Form extraction and processing
* Compliance control with document redaction
* Table extraction and processing
* PDF document processing
  
### Amazon Textract
Before I get started with the use cases, let me review and introduce some of the core features. Amazon Textract goes beyond simple optical character recognition (OCR) to also identify the contents of fields in forms and information stored in tables. This allows you to use Amazon Textract to instantly "read" virtually any type of document and accurately extract text and data without the need for any manual effort or custom code.

The following images show an example document and corresponding extracted text, form, and table data using Amazon Textract in the AWS Management Console.

![image info](/img/awscloud/8/textract-ga-1.gif)

The following image shows the lines extracted as raw text from the document.

![image info](/img/awscloud/8/textract-ga-2.gif)


