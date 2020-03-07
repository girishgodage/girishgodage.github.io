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

![image info](/img/awscloud/8/textract-ga-6.gif)

The following code example shows how to use a few lines of code to send this sample image to Amazon Textract and get a JSON response back. You then iterate over the blocks in JSON and print the detected text, as shown below.

```python
import boto3

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "simple-document-image.jpg"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.detect_document_text(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    })

#print(response)

# Print detected text
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')

```
The following JSON response is what you receive from Amazon Textract, with blocks representing detected text in the document.

```
  {
    "Blocks": [
        {
            "Geometry": {
                "BoundingBox": {
                    "Width": 1.0, 
                    "Top": 0.0, 
                    "Left": 0.0, 
                    "Height": 1.0
                }, 
                "Polygon": [
                    {
                        "Y": 0.0, 
                        "X": 0.0
                    }, 
                    {
                        "Y": 0.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 1.0
                    }, 
                    {
                        "Y": 1.0, 
                        "X": 0.0
                    }
                ]
            }, 
            "Relationships": [
                {
                    "Type": "CHILD", 
                    "Ids": [
                        "2602b0a6-20e3-4e6e-9e46-3be57fd0844b", 
                        "82aedd57-187f-43dd-9eb1-4f312ca30042", 
                        "52be1777-53f7-42f6-a7cf-6d09bdc15a30", 
                        "7ca7caa6-00ef-4cda-b1aa-5571dfed1a7c"
                    ]
                }
            ], 
            "BlockType": "PAGE", 
            "Id": "8136b2dc-37c1-4300-a9da-6ed8b276ea97"
        }..... 
        
    ], 
    "DocumentMetadata": {
        "Pages": 1
    }
}


```
The following image shows the output of the detected 

![image info](/img/awscloud/8/textract-ga-7.gif)

### Multi-column detection and reading order
Traditional OCR solutions read left to right, do not detect multiple columns, and end up generating incorrect reading order for multi-column documents. In addition to detecting text, Amazon Textract provides additional geometry information that can be used to detect multiple columns and print the text in reading order.
The following image is a two-column document. Similar to the earlier example, the image is not good quality but Amazon Textract still performs well.

![image info](/img/awscloud/8/textract-ga-8-2.gif)

The following example code shows processing the document with Amazon Textract and taking advantage of geometry information to print the text in reading order.

```python

import boto3

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "two-column-image.jpg"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.detect_document_text(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    })

#print(response)

# Detect columns and print lines
columns = []
lines = []
for item in response["Blocks"]:
      if item["BlockType"] == "LINE":
        column_found=False
        for index, column in enumerate(columns):
            bbox_left = item["Geometry"]["BoundingBox"]["Left"]
            bbox_right = item["Geometry"]["BoundingBox"]["Left"] + item["Geometry"]["BoundingBox"]["Width"]
            bbox_centre = item["Geometry"]["BoundingBox"]["Left"] + item["Geometry"]["BoundingBox"]["Width"]/2
            column_centre = column['left'] + column['right']/2

            if (bbox_centre > column['left'] and bbox_centre < column['right']) or (column_centre > bbox_left and column_centre < bbox_right):
                #Bbox appears inside the column
                lines.append([index, item["Text"]])
                column_found=True
                break
        if not column_found:
            columns.append({'left':item["Geometry"]["BoundingBox"]["Left"], 'right':item["Geometry"]["BoundingBox"]["Left"] + item["Geometry"]["BoundingBox"]["Width"]})
            lines.append([len(columns)-1, item["Text"]])

lines.sort(key=lambda x: x[0])
for line in lines:
    print (line[1])

```
The following image shows the output of the detected text in the correct reading order.

![image info](/img/awscloud/8/textract-ga-9.gif )