---
title: Authenticate applications through facial recognition with Amazon Cognito and
  Amazon Rekognition
date: 2020-01-25 10:41:00 Z
permalink: "/blog/authenticate-applications-through-facial-recognition-with-amazon-cognito-and-amazon-rekognition"
categories:
- AWSCloud
tags:
- learning
summary: we implemented an authentication mechanism using facial recognition using
  the custom authentication flows provided by Amazon Cognito combined with Amazon
  Rekognition. Depending on your organization and workload security criteria and requirements,
  this scenario might work from both security and user experience point of views.
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-25-AmazonForecast
---

## Authenticate applications through facial recognition with Amazon Cognito and Amazon Rekognition

With increased use of different applications, social networks, financial platforms, emails and cloud storage solutions, managing different passwords and credentials can become a burden. In many cases, sharing one password across all these applications and platforms is just not possible. Different security standards may be required, such as passwords composed by only numeric characters, password renewal policies, and providing security questions.

But what if you could enhance the ways users authenticate themselves in their application in a more convenient, simpler and above everything, more secure way? In this post, I will show how to leverage Amazon Cognito user pools to customize your authentication flows and allow logging into your applications using Amazon Rekognition for facial recognition using a sample application.

## Solution Overview 

We will build a Mobile or Web application that allows users to sign in using an email and require the user to upload a document containing his or her photo. We will use the **AWS Amplify Framework** to integrate our Front-End application with **Amazon S3** and store this image in a secure and encrypted bucket.  Our solution will trigger a Lambda function for each new image uploaded to this bucket so that we can index the images inside Amazon Rekognition and save the metadata in a **DynamoDB** table for later queries.

For authentication, this solution uses Amazon Cognito User Pools combined with Lambda functions to customize the authentication flows together with the **Amazon Rekognition CompareFaces** API to identify the confidence level between user photos provided during Sign Up and Sign In. Here is the architecture of the solution:

![image info](/img/awscloud/12/cognito-rekognition-architecure.png)

Here's a step-wise description of the above data-flow architecture diagram:

  **1.** User signs up into the Cognito User Pool.

  **2.** User uploads – during Sign Up – a document image containing his/her photo and name, to an S3 Bucket (e.g. Passport).
  
  **3.** A Lambda function is triggered containing the uploaded image as payload.
  
  **4.** The function first indexes the image in a specific Amazon Rekognition Collection to store these user documents.

  **5.** The same function then persists in a DynamoDB table as the indexed image metadata, together with the email registered in Amazon Cognito User Pool for later queries.

  **6.** User enters an email in the custom Sign In page, which makes a request to Cognito User Pool.
  
  **7.** Amazon Cognito User Pool triggers the "Define Auth Challenge" trigger that determines which custom challenges are to be created at this moment.

  **8.** The User Pool then invokes the "Create Auth Challenge" trigger. This trigger queries the DynamoDB table for the user containing the given email id to retrieve its indexed photo from the Amazon Rekognition Collection.

  **9.** The User Pool invokes the "Verify Auth Challenge" trigger. This verifies if the challenge was indeed successfully completed; if it finds an image, it will compare it with the photo taken during Sign In to measure its confidence between both the images.

  **10.** The User Pool, once again, invokes the "Define Auth Challenge" trigger that verifies if the challenge was answered. No no further challenges are created, if the 'Define Auth challenge' is able to verify the user-supplied answer. The trigger response, back to the User Poll will include an "issueTokens:true" attribute to authenticate itself and finally issue the user a JSON Web Token (JWT) (see step 6).

### Serverless Application and the different Lambdas invoked

The following solution is available as a Serverless application. You can deploy it directly from AWS Serverless Application Repository. Core parts of this implementation are:

* Users are required to use a valid email as user name.
* The solution includes a Cognito App Client configured to "Only allow custom authentication" as Amazon Cognito requires a password for user sign up. We are creating a random password to these users, since we don't want them to Sign In using these passwords later.
* We use two Amazon S3 Buckets: one to store document images uploaded during Sign Up and one to store user photos taken when Signing In for face comparisons.
* We use two different Lambda runtimes (Python and Node.js) to demonstrate how AWS Serverless Application Model (SAM) handles multiple runtimes in the same project and development environment for the developer's perspective.

The following Lambda functions are triggered to implement the images indexing in Amazon Rekognition and customize Amazon Cognito User Pools custom authentication challenges:

**1. Create Rekognition Collection** (Python 3.6)– This Lambda function gets triggered only once, at the beginning of deployment, to create a Custom Collection in Amazon Rekognition to index documents for user Sign Ups.

**2. Index Images** (Python 3.6) – This Lambda function gets triggered for each new document upload to Amazon S3 during Sign Up and indexes the uploaded document in the Amazon Rekognition Collection (mentioned in the previous step) and then persists its metadata into DynamoDB.

**3.Define Auth Challenge** (Node.js 8.10) – This Lambda function tracks the custom authentication flow, which is comparable to a decider function in a state machine. It determines which challenges are presented, in what order, to the user. At the end, it reports back to the user pool if the user succeeded or failed authentication. The Lambda function is invoked at the start of the custom authentication flow and also after each completion of the "Verify Auth Challenge Response" trigger.

**4. Create Auth Challenge** (Node.js 8.10) – This Lambda function gets invoked, based on the instruction of the "Define Auth Challenge" trigger, to create a unique challenge for the user. We will use this function to query DynamoDB for existing user records and if their given metadata are valid.

**5. Verify Auth Challenge Response** (Node.js 8.10) – This Lambda function gets invoked by the user pool when the user provides the answer to the challenge. Its only job is to determine if that answer is correct. In this case, it compares both images provided during Sign Up and Sign In, using the Amazon Rekognition CompareFaces API and considers a API responses containing a confidence level equals or greater than 90% as a valid challenge response

In the sections below, let's step through the code for the different Lambda functions we described above.

### 1. Create an Amazon Rekognition Collection
   
As described above, this function creates a Collection in Amazon Rekognition that will later receive user photos uploaded during Sign Up.

```python
import boto3
import os

def handler(event, context):

    maxResults=1
    collectionId=os.environ['COLLECTION_NAME']
    
    client=boto3.client('rekognition')

    #Create a collection
    print('Creating collection:' + collectionId)
    response=client.create_collection(CollectionId=collectionId)
    print('Collection ARN: ' + response['CollectionArn'])
    print('Status code: ' + str(response['StatusCode']))
    print('Done...')
    return response
```

### 2. Index Images into Amazon Rekognition
   
This function is responsible for receiving uploaded images during the sign up from users and index the images in the Amazon Rekognition Collection created by the Lambda described above to persist its metadata in an Amazon Dynamodb table.

```python
from __future__ import print_function
import boto3
from decimal import Decimal
import json
import urllib
import os

dynamodb = boto3.client('dynamodb')
s3 = boto3.client('s3')
rekognition = boto3.client('rekognition')

# --------------- Helper Functions ------------------

def index_faces(bucket, key):

    response = rekognition.index_faces(
        Image={"S3Object":
            {"Bucket": bucket,
            "Name": key}},
            CollectionId=os.environ['COLLECTION_NAME'])
    return response
    
def update_index(tableName,faceId, fullName):
    response = dynamodb.put_item(
        TableName=tableName,
        Item={
            'RekognitionId': {'S': faceId},
            'FullName': {'S': fullName}
            }
        ) 
    
# --------------- Main handler ------------------

def handler(event, context):

    # Get the object from the event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = urllib.parse.unquote_plus(
        event['Records'][0]['s3']['object']['key'].encode('utf8'))

    try:

        # Calls Amazon Rekognition IndexFaces API to detect faces in S3 object 
        # to index faces into specified collection
        
        response = index_faces(bucket, key)
        
        # Commit faceId and full name object metadata to DynamoDB
        
        if response['ResponseMetadata']['HTTPStatusCode'] == 200:
            faceId = response['FaceRecords'][0]['Face']['FaceId']
            ret = s3.head_object(Bucket=bucket,Key=key)
            email = ret['Metadata']['email']
            update_index(os.environ['COLLECTION_NAME'],faceId, email) 
        return response
    except Exception as e:
        print("Error processing object {} from bucket {}. ".format(key, bucket))
        raise e

```

## 3. Define Auth Challenge Function

This is the decider function that manages the authentication flow. In the session array that's provided to this Lambda function (event.request.session), the entire state of the authentication flow is present. If it's empty, it means the custom authentication flow just started. If it has items, the custom authentication flow is underway, i.e. a challenge was presented to the user, the user provided an answer, and it was verified to be right or wrong. In either case, the decider function has to decide what to do next:

```node.js
exports.handler = async (event, context) => {

    console.log("Define Auth Challenge: " + JSON.stringify(event));

    if (event.request.session &&
        event.request.session.length >= 3 &&
        event.request.session.slice(-1)[0].challengeResult === false) {
        // The user provided a wrong answer 3 times; fail auth
        event.response.issueTokens = false;
        event.response.failAuthentication = true;
    } else if (event.request.session &&
        event.request.session.length &&
        event.request.session.slice(-1)[0].challengeResult === true) {
        // The user provided the right answer; succeed auth
        event.response.issueTokens = true;
        event.response.failAuthentication = false;
    } else {
        // The user did not provide a correct answer yet; present challenge
        event.response.issueTokens = false;
        event.response.failAuthentication = false;
        event.response.challengeName = 'CUSTOM_CHALLENGE';
    }

    return event;
}

```

### 4. Create Auth Challenge Function

This function queries DynamoDB for a record containing the given e-mail to retrieve its image ID inside Amazon Rekognition Collection and define as a challenge that the user must provide a photo that relates to the same person.


```node.js
const aws = require('aws-sdk');
const dynamodb = new aws.DynamoDB.DocumentClient();

exports.handler = async (event, context) => {

    console.log("Create auth challenge: " + JSON.stringify(event));

    if (event.request.challengeName == 'CUSTOM_CHALLENGE') {
        event.response.publicChallengeParameters = {};

        let answer = '';
        // Querying for Rekognition ids for the e-mail provided
        const params = {
            TableName: process.env.COLLECTION_NAME,
            IndexName: "FullName-index",
            ProjectionExpression: "RekognitionId",
            KeyConditionExpression: "FullName = :userId",
            ExpressionAttributeValues: {
                ":userId": event.request.userAttributes.email
            }
        }
        
        try {
            const data = await dynamodb.query(params).promise();
            data.Items.forEach(function (item) {
                
                answer = item.RekognitionId;

                event.response.publicChallengeParameters.captchaUrl = answer;
                event.response.privateChallengeParameters = {};
                event.response.privateChallengeParameters.answer = answer;
                event.response.challengeMetadata = 'REKOGNITION_CHALLENGE';
                
                console.log("Create Challenge Output: " + JSON.stringify(event));
                return event;
            });
        } catch (err) {
            console.error("Unable to query. Error:", JSON.stringify(err, null, 2));
            throw err;
        }
    }
    return event;
}

```
### 5. Verify Auth Challenge Response Function

This function verifies within Amazon Rekognition if it can find an image with the confidence level equals or over 90% compared to the image uploaded during Sign In, and if this image refers to the user claims to be through the given e-mail address.

```Node.js
var aws = require('aws-sdk');
var rekognition = new aws.Rekognition();

exports.handler = async (event, context) => {

    console.log("Verify Auth Challenge: " + JSON.stringify(event));
    let userPhoto = '';
    event.response.answerCorrect = false;

    // Searching existing faces indexed on Rekognition using the provided photo on s3

    const objectName = event.request.challengeAnswer;
    const params = {
        "CollectionId": process.env.COLLECTION_NAME,
        "Image": {
            "S3Object": {
                "Bucket": process.env.BUCKET_SIGN_UP,
                "Name": objectName
            }
        },
        "MaxFaces": 1,
        "FaceMatchThreshold": 90
    };
    try {
        const data = await rekognition.searchFacesByImage(params).promise();

        // Evaluates if Rekognition was able to find a match with the required 
        // confidence threshold

        if (data.FaceMatches[0]) {
            console.log('Face Id: ' + data.FaceMatches[0].Face.FaceId);
            console.log('Similarity: ' + data.FaceMatches[0].Similarity);
            userPhoto = data.FaceMatches[0].Face.FaceId;
            if (userPhoto) {
                if (event.request.privateChallengeParameters.answer == userPhoto) {
                    event.response.answerCorrect = true;
                }
            }
        }
    } catch (err) {
        console.error("Unable to query. Error:", JSON.stringify(err, null, 2));
        throw err;
    }
    return event;
}

```

### The Front End Application 
Now that we've stepped through all the Lambdas, let's create a custom Sign In page, in order to orchestrate and test our scenario. You can use AWS Amplify Framework to integrate your Sign In page to Amazon Cognito and the photo uploads to Amazon S3.

The AWS Amplify Framework allows you to implement your application using your favourite framework (React, Angular, Vue, HTML/JavaScript, etc). You can customize the snippets below as per your requirements. The snippets below demonstrate how to import and initialize AWS Amplify Framework on React:

```
import Amplify from 'aws-amplify';

Amplify.configure({
  Auth: {
    region: 'your region',
    userPoolId: 'your userPoolId',
    userPoolWebClientId: 'your clientId',
  },
  Storage: { 
    region: 'your region', 
    bucket: 'your sign up bucket'
  }
});
```

### Signing Up

For users to be able to sign themselves up, as mentioned above, we will "generate" a random password on their behalf since it is required by Amazon Cognito for user sign up. However, once we create our Cognito User Pool Client, we ensure that authentication only happens following the custom authentication flow – never using user and password.

```
import { Auth } from 'aws-amplify';

signUp = async event => {
  const params = {
    username: this.state.email,
    password: getRandomString(30),
    attributes: {
      name: this.state.fullName
    }
  };
  await Auth.signUp(params);
};

function getRandomString(bytes) {
  const randomValues = new Uint8Array(bytes);
  window.crypto.getRandomValues(randomValues);
  return Array.from(randomValues).map(intToHex).join('');
}

function intToHex(nr) {
  return nr.toString(16).padStart(2, '0');
}
```
### Signing in
Starts the custom authentication flow to the user.

```
import { Auth } from "aws-amplify";

signIn = () => { 
    try { 
        user = await Auth.signIn(this.state.email);
        this.setState({ user });
    } catch (e) { 
        console.log('Oops...');
    } 
};
```

### Answering the Custom Challenge

In this step, we open the camera through Browser to take a user photo and then upload it to Amazon S3, so we can start the face comparison.

```
import Webcam from "react-webcam";

// Instantiate and set webcam to open and take a screenshot
// when user is presented with a custom challenge

/* Webcam implementation goes here */


// Retrieves file uploaded to S3 and sends as a File to Rekognition 
// as answer for the custom challenge
dataURLtoFile = (dataurl, filename) => {
  var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
      bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
  while(n--){
      u8arr[n] = bstr.charCodeAt(n);
  }
  return new File([u8arr], filename, {type:mime});
};

sendChallengeAnswer = () => {

    // Capture image from user camera and send it to S3
    const imageSrc = this.webcam.getScreenshot();
    const attachment = await s3UploadPub(dataURLtoFile(imageSrc, "id.png"));
    
    // Send the answer to the User Pool
    const answer = `public/${attachment}`;
    user = await Auth.sendCustomChallengeAnswer(cognitoUser, answer);
    this.setState({ user });
    
    try {
        // This will throw an error if the user is not yet authenticated:
        await Auth.currentSession();
    } catch {
        console.log('Apparently the user did not enter the right code');
    }
    
};

```

## Conclusion
In this blog post, we implemented an authentication mechanism using facial recognition using the custom authentication flows provided by Amazon Cognito combined with Amazon Rekognition. Depending on your organization and workload security criteria and requirements, this scenario might work from both security and user experience point of views. Additionally, we can enhance the security factor by chaining multiple Auth Challenges not only based on the user photo, but also a combination of Liveness Detection, a combination of their document numbers used for signing up and other additional MFA's.

Since this is an entirely Serverless-based solution, you can customize it as your requirements arise using AWS Lambda functions. You can read more on custom authentication in our developer guide.