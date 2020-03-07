---
title: Automatically extract text and structured data from documents with Amazon Textract
date: 2020-01-20 10:41:00 Z
permalink: "/blog/automatically-extract-text-and-structured-data-from-documents-with-amazon-textract"
categories:
- AWSCloud
tags:
- learning
summary: Documents are a primary tool for record keeping, communication, collaboration,
  and transactions across many industries, including financial, medical, legal, and
  real estate. The millions of mortgage applications and hundreds of millions of W2
  tax forms processed each year are just a few examples of such documents. A lot of
  information is locked in unstructured documents.It usually requires time-consuming
  and complex processes to enable search and discovery, business process automation,
  and compliance control for these documents.In this post, I show how you can take
  advantage of Amazon Textract to automatically extract text and data from scanned
  documents without any machine learning (ML) experience.
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

### Natural language processing and document classification

Customer emails, support tickets, product reviews, social media, even advertising copy all represent insights into customer sentiment that can be put to work for your business. 

A lot of such content contains images or scanned versions of documents. After text is extracted from these documents, you can use Amazon Comprehend to detect sentiment, entities, key phrases, syntax and topics. You can also train Amazon Comprehend to detect custom entities based on your business domain. These insights can then be used to classify documents, automate business process workflows, and ensure compliance.

The following example code shows processing the first image sample used earlier with Amazon Textract to extract text and then using Amazon Comprehend to detect sentiment and entities.

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

# Print text
print("\nText\n========")
text = ""
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')
        text = text + " " + item["Text"]

# Amazon Comprehend client
comprehend = boto3.client('comprehend')

# Detect sentiment
sentiment =  comprehend.detect_sentiment(LanguageCode="en", Text=text)
print ("\nSentiment\n========\n{}".format(sentiment.get('Sentiment')))

# Detect entities
entities =  comprehend.detect_entities(LanguageCode="en", Text=text)
print("\nEntities\n========")
for entity in entities["Entities"]:
    print ("{}\t=>\t{}".format(entity["Type"], entity["Text"]))

```

The following image shows the output text along with the text analysis from Amazon Comprehend. You can see that it found the sentiment to be "Neutral" and detected "Amazon" as an organization, "Seattle, WA" as a location and "July 5th, 1994" as a date, along with other entities.

![image info](/img/awscloud/8/textract-ga-10.gif)


### Natural language processing for medical documents
One of the important ways to improve patient care and accelerate clinical research is by understanding and analyzing the insights and relationships that are "trapped" in free-form medical text. These can include hospital admission notes and a patient's medical history.
In this example, use the following document to extract text using Amazon Textract. You then use Amazon Comprehend Medical to extract medical entities, such as medical condition, medication, dosage, strength, and protected health information (PHI).

![image info](/img/awscloud/8/patient-notes.gif)

The following example code shows how different medical entities are detected.

```python
import boto3

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "medical-notes.png"

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

# Print text
print("\nText\n========")
text = ""
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')
        text = text + " " + item["Text"]

# Amazon Comprehend client
comprehend = boto3.client('comprehendmedical')

# Detect medical entities
entities =  comprehend.detect_entities(Text=text)
print("\nMidical Entities\n========")
for entity in entities["Entities"]:
    print("- {}".format(entity["Text"]))
    print ("   Type: {}".format(entity["Type"]))
    print ("   Category: {}".format(entity["Category"]))
    if(entity["Traits"]):
        print("   Traits:")
        for trait in entity["Traits"]:
            print ("    - {}".format(trait["Name"]))
    print("\n")
```

The following image and text block shows the output of the detected text with information categorized by type. It detected "40yo" as the age with category "Protected Health Information". It also detected different medical conditions, including sleeping trouble, rash, inferior turbinates, erythematous eruption, and others. It recognized different medications and anatomy information.

![image info](/img/awscloud/8/textract-ga-12.gif)

```python 
Medical Entities
========
- 40yo
   Type: AGE
   Category: PROTECTED_HEALTH_INFORMATION
- Sleeping trouble
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SYMPTOM
- Clonidine
   Type: GENERIC_NAME
   Category: MEDICATION
- Rash
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SYMPTOM
- face
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- leg
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- Vyvanse
   Type: BRAND_NAME
   Category: MEDICATION
- Clonidine
   Type: GENERIC_NAME
   Category: MEDICATION
- HEENT
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- Boggy inferior turbinates
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SIGN
- inferior
   Type: DIRECTION
   Category: ANATOMY
- turbinates
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- oropharyngeal lesion
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SIGN
    - NEGATION
- Lungs
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- clear Heart
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SIGN
- Heart
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- Regular rhythm
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SIGN
- Skin
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
- erythematous eruption
   Type: DX_NAME
   Category: MEDICAL_CONDITION
   Traits:
    - SIGN
- hairline
   Type: SYSTEM_ORGAN_SITE
   Category: ANATOMY
```

### Document translation
Many organizations localize content for international users, such as websites and applications. They must translate large volumes of documents efficiently. You can use Amazon Textract along with Amazon Translate to extract text and data and then translate them into other languages.

The following code example shows translating the text in the first image to German.

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

# Amazon Translate client
translate = boto3.client('translate')

print ('')
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')
        result = translate.translate_text(Text=item["Text"], SourceLanguageCode="en", TargetLanguageCode="de")
        print ('\033[92m' + result.get('TranslatedText') + '\033[0m')
    print ('')
```
The following image shows the output of the detected text, translated to German line by line.

![image info](/img/awscloud/8/textract-ga-13.gif)

### Search and discovery

Extracting structured data from documents and creating a smart index using **Amazon Elasticsearch Service (Amazon ES)** allows you to search through millions of documents quickly. For example, *a mortgage company could use Amazon Textract to process millions of scanned loan applications in a matter of hours and have the extracted data indexed in Amazon ES. This would allow them to create search experiences like searching for loan applications where the applicant name is John Doe, or searching for contracts where the interest rate is 2 percent.*

The following code example shows how you can extract text from the first image, store it in Amazon ES, and then search it using Kibana. You can also build a custom UI experience by taking advantage of the Amazon ES APIs. Later in the post, as you learn how to extract forms and tables, that structured data can then be indexed similarly to enable smart search.

```python
import boto3
from elasticsearch import Elasticsearch, RequestsHttpConnection
from requests_aws4auth import AWS4Auth

def indexDocument(bucketName, objectName, text):

    # Update host with endpoint of your Elasticsearch cluster
    #host = "search--xxxxxxxxxxxxxx.us-east-1.es.amazonaws.com
    host = "searchxxxxxxxxxxxxxxxx.us-east-1.es.amazonaws.com"
    region = 'us-east-1'

    if(text):
        service = 'es'
        ss = boto3.Session()
        credentials = ss.get_credentials()
        region = ss.region_name

        awsauth = AWS4Auth(credentials.access_key, credentials.secret_key, region, service, session_token=credentials.token)

        es = Elasticsearch(
            hosts = [{'host': host, 'port': 443}],
            http_auth = awsauth,
            use_ssl = True,
            verify_certs = True,
            connection_class = RequestsHttpConnection
        )

        document = {
            "name": "{}".format(objectName),
            "bucket" : "{}".format(bucketName),
            "content" : text
        }

        es.index(index="textract", doc_type="document", id=objectName, body=document)

        print("Indexed document: {}".format(objectName))

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
text = ""
for item in response["Blocks"]:
    if item["BlockType"] == "LINE":
        print ('\033[94m' +  item["Text"] + '\033[0m')
        text += item["Text"]

indexDocument(s3BucketName, documentName, text)

# You can view index documents in Kibana Dashboard
```

The following image shows the output of extracted text in Kibana search results.

![image info](/img/awscloud/8/textract-ga-14.gif)

### Form extraction and processing

Amazon Textract can provide the inputs required to automatically process forms without human intervention. For example, *a bank could write code to read PDFs of loan applications. The information contained in the document could be used to initiate all of the necessary background and credit checks to approve the loan so that customers can get instant results for their application rather than having to wait several days for manual review and validation.
The following image is an employment application with form fields and a table.*

![image info](/img/awscloud/8/textract-ga-15.gif)

The following code example shows how to extract forms from the employment application and process different fields.

```python
import boto3
from trp import Document

# Document
s3BucketName = "ki-textract-demo-docs"
documentName = "employmentapp.png"

# Amazon Textract client
textract = boto3.client('textract')

# Call Amazon Textract
response = textract.analyze_document(
    Document={
        'S3Object': {
            'Bucket': s3BucketName,
            'Name': documentName
        }
    },
    FeatureTypes=["FORMS"])

#print(response)

doc = Document(response)

for page in doc.pages:
    # Print fields
    print("Fields:")
    for field in page.form.fields:
        print("Key: {}, Value: {}".format(field.key, field.value))

    # Get field by key
    print("\nGet Field by Key:")
    key = "Phone Number:"
    field = page.form.getFieldByKey(key)
    if(field):
        print("Key: {}, Value: {}".format(field.key, field.value))

    # Search fields by key
    print("\nSearch Fields:")
    key = "address"
    fields = page.form.searchFieldsByKey(key)
    for field in fields:
        print("Key: {}, Value: {}".format(field.key, field.value))
```

The following image is the output of detected form for the employment application.


![image info](/img/awscloud/8/textract-ga-19.gif)
