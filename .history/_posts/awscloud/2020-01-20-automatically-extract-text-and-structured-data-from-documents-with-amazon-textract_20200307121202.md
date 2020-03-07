---
title: Automatically extract text and structured data from documents with Amazon Textract
date: 2020-01-20 10:41:00 Z
permalink: "/blog/automatically-extract-text-and-structured-data-from-documents-with-amazon-textract"
categories:
- AWSCloud
tags:
- learning
summary: Documents are a primary tool for record keeping, communication, collaboration, and transactions across many industries, including financial, medical, legal, and real estate. The millions of mortgage applications and hundreds of millions of W2 tax forms processed each year are just a few examples of such documents. A lot of information is locked in unstructured documents.It usually requires time-consuming and complex processes to enable search and discovery, business process automation, and compliance control for these documents.In this post, I show how you can take advantage of Amazon Textract to automatically extract text and data from scanned documents without any machine learning (ML) experience.
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

The following image shows the extracted form fields and their corresponding values.


![image info](/img/awscloud/8/textract-ga-3.gif)

The following image shows the extracted table, cells, and the text in those cells.

![image info](/img/awscloud/8/textract-ga-4.gif)

To quickly download a zip file containing the output, choose Download results. You can choose various formats, including raw JSON, text, and CSV files for forms and tables.

![image info](/img/awscloud/8/textract-ga-5.gif)

In addition to the detected content, Amazon Textract provides additional information, like confidence scores and bounded boxes for detected elements. It gives you control on how you consume extracted content and integrate it into various business applications.

Amazon Textract provides both **synchronous and asynchronous** API actions to extract document text and analyze the document text data. 

**Synchronous APIs** can be used for *single-page* documents and low latency use cases such as mobile capture. **Asynchronous APIs** can be used for *multi-page* documents such as PDF documents with thousands of pages. For more information, see the Amazon Textract API Reference.

### Use cases
Now, write some code to take advantage of Amazon Textract API operations using the AWS SDK and see how easy it is to build power-smart applications. I will also use the JSON Parser Library for some of the below use cases.

### Text detection from documents

I start with a simple example on how to detect text from a document. Use the following image as an input document to Amazon Textract. As you can see, the sample image is not of good quality, but Amazon Textract can still detect the text with accuracy.