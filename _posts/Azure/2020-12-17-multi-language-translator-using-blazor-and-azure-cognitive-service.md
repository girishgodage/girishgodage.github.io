---
title: Multi-Language Translator Using Blazor And Azure Cognitive Services
date: 2020-12-17 16:55:00 Z
permalink: "/blog/multi-Language-Translator-using-blazor-AzureCognitiveServices"
categories:
- AzureAI
tags:
- learning
summary: In this article, we will create a multilanguage translator using Blazor and the Translate Text Azure Cognitive Service. 
image: "/img/azure/11/output.gif"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-12-17-multi-Language-Translator-using-blazor-AzureCognitiveServices
---

## Introduction

In this article, we will create a multilanguage translator using Blazor and the Translate Text Azure Cognitive Service. This translator will be able to translate between all the languages supported by the Translate Text API. Currently, the Translate Text API supports more than 60 languages for translation. The application will accept the text to translate and the target language as the input and returns the translated text and the detected language for the input text as the output.

Take a look at the output shown below.

![image info](/img/azure/11/output.gif)

## Prerequisites
  * Install the latest .NET Core 3.1 SDK from https://dotnet.microsoft.com/download/dotnet-core/3.1
  * Install the latest version of Visual Studio 2019 from https://visualstudio.microsoft.com/downloads/
 * An Azure subscription account. You can create a free Azure account  at https://azure.microsoft.com/en-in/free/


## Source Code
You can get the source code from [GitHub](https://github.com/girishgodage/Blazor-Translator-Azure-Cognitive-Services).

## Create the Azure Translator Text Cognitive Services resource

Log in to the Azure portal and search for the cognitive services in the search bar and click on the result. Refer to the image shown below.

![image info](/img/azure/9/SearchCognitive.png)

On the next screen, click on the Add button. It will open the cognitive services marketplace page. Search for the Translator Text in the search bar and click on the search result. It will open the Translator Text API page. Click on the Create button to create a new Translator Text resource. Refer to the image shown below.

![image info](/img/azure/11/CreateTranslatorService.png)


On the Create page, fill in the details as indicated below.

  * **Name**: Give a unique name for your resource.
  * **Subscription**: Select the subscription type from the dropdown.
  * **Pricing tier**: Select the pricing tier as per your choice.
  * **Resource group**: Select an existing resource group or create a new one.

Click on the Create button. Refer to the image shown below.

![image info](/img/azure/11/CreateTranslatorService_1.png)

![image info](/img/azure/11/CreateTranslatorService_2.png)


After your resource is successfully deployed, click on the “Go to resource” button. You can see the Key and the endpoint for the newly created Computer Vision resource. Refer to the image shown below.

![image info](/img/azure/11/TranslatorKeyNEndPoint.png)


Make a note of the **key**, we will be using this in the latter part of this article to request the translations from the Translator Text API. The values are masked here for privacy.

## Create a Server-Side Blazor Application

Open Visual Studio 2019, click on “Create a new project”. Select “Blazor App” and click on the “Next” button. Refer to the image shown below.

![image info](/img/azure/10/Create_BlazorApp.png)

On the next window, put the project name as <span style="color:yellow;background-color:black;">BlazorTranslator</span> and click on the “Create” button. The next window will ask you to select the type of Blazor app. Select <span style="color:yellow;background-color:black;">Blazor Server App </span> and click on the Create button to create a new server-side Blazor application. Refer to the image shown below.

![image info](/img/azure/10/Create_BlazorApp_1.png)


## Create the Models

Right-click on the <span style="color:yellow;background-color:black;">BlazorTranslator</span> project and select Add >> New Folder. Name the folder as Models. Again, right-click on the Models folder and select Add >> Class to add a new class file. Put the name of your class as <span style="color:yellow;background-color:black;">LanguageDetails.cs</span> and click Add.

Open <span style="color:yellow;background-color:black;">LanguageDetails.cs</span> and put the following code inside it.

```code
 namespace BlazorTranslator.Models
{
    public class LanguageDetails
    {
        public string Name { get; set; }
        public string NativeName { get; set; }
        public string Dir { get; set; }
    }
}

```

Similarly, add a new class file <span style="color:yellow;background-color:black;">TestResult.cs </span> and put the following code inside it.

```code
 using System;
namespace BlazorTranslator.Models
{
    public class TextResult
    {
        public string Text { get; set; }
        public string Script { get; set; }
    }
}

```

Add a new class file Translation.cs and put the following code inside it.

```code

    namespace BlazorTranslator.Models
    {
        public class Translation
        {
            public string Text { get; set; }
            public TextResult Transliteration { get; set; }
            public string To { get; set; }
        }
    }

```

Create a class file <span style="color:yellow;background-color:black;">DetectedLanguage.cs</span> and put the following code inside it.

```code
    namespace BlazorTranslator.Models
    {
        public class DetectedLanguage
        {
            public string Language { get; set; }
            public float Score { get; set; }
        }
    }

```

Create a class file <span style="color:yellow;background-color:black;">TranslationResult.cs</span> and put the following code inside it.

```code

    namespace BlazorTranslator.Models
    {
        public class TranslationResult
        {
            public DetectedLanguage DetectedLanguage { get; set; }
            public TextResult SourceText { get; set; }
            public Translation[] Translations { get; set; }
        }
    }

```

Finally, create the class file <span style="color:yellow;background-color:black;">AvailableLanguage.cs</span> and put the following code inside it.

```code
    using System.Collections.Generic;
    namespace BlazorTranslator.Models
    {
        public class AvailableLanguage
        {
        public Dictionary<string, LanguageDetails> Translation { get; set; }
        }
    }

```

## Create the Translation service

Right-click on the <span style="color:yellow;background-color:black;">BlazorTranslator/Data</span> folder and select Add >> Class to add a new class file. Put the name of the file as <span style="color:yellow;background-color:black;">TranslationService.cs</span> and click on Add. Open <span style="color:yellow;background-color:black;">TranslationService.cs</span> file and put the following code inside it.

```code

    using BlazorTranslator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    namespace BlazorTranslator.Data
    {
        
        public class TranslationService
        {
            private static readonly string location = "eastus";
            
            public async Task<TranslationResult[]> GetTranslatation(string textToTranslate, string targetLanguage)
            {
                string subscriptionKey = "52937b775b454b65830ac59e5dc22d9d";
                string apiEndpoint = "https://api.cognitive.microsofttranslator.com/";
                string route = $"/translate?api-version=3.0&to={targetLanguage}";
                string requestUri = apiEndpoint + route;
                TranslationResult[] translationResult = await TranslateText(subscriptionKey, requestUri, textToTranslate);
                return translationResult;
            }
            async Task<TranslationResult[]> TranslateText(string subscriptionKey, string requestUri, string inputText)
            {
                object[] body = new object[] { new { Text = inputText } };
                var requestBody = JsonConvert.SerializeObject(body);
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Post;
                    request.RequestUri = new Uri(requestUri);
                    request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                    request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                    request.Headers.Add("Ocp-Apim-Subscription-Region", location);
                    HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
                    string result = await response.Content.ReadAsStringAsync();
                    TranslationResult[] deserializedOutput = JsonConvert.DeserializeObject<TranslationResult[]>(result);
                    return deserializedOutput;
                }
            }
            public async Task<AvailableLanguage> GetAvailableLanguages()
            {
                string endpoint = "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation";
                var client = new HttpClient();
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Get;
                    request.RequestUri = new Uri(endpoint);
                    var response = await client.SendAsync(request).ConfigureAwait(false);
                    string result = await response.Content.ReadAsStringAsync();
                    AvailableLanguage deserializedOutput = JsonConvert.DeserializeObject<AvailableLanguage>(result);
                    return deserializedOutput;
                }
            }
        }
    }

```

We have defined a <span style="color:yellow;background-color:black;">GetTranslatation</span> method which will accept two parameters – the text to translate and the target language. We will set the subscription key for the Azure Translator Text cognitive service and define a variable for the global endpoint for Translator Text. The request URL contains the API endpoint along with the target language.

Inside the <span style="color:yellow;background-color:black;">TranslateText</span> method, we will create a new <span style="color:yellow;background-color:black;">HttpRequestMessage</span>. This HTTP request is a Post request. We will pass the subscription key in the header of the request. The Translator Text API returns a JSON object, which will be deserialized to an array of type <span style="color:yellow;background-color:black;">TranslationResult</span>. The output contains the translated text as well as the language detected for the input text.

The <span style="color:yellow;background-color:black;">GetAvailableLanguages</span> method will return the list of all the language supported by the Translate Text API. We will set the request URI and create a <span style="color:yellow;background-color:black;">HttpRequestMessage</span> which will be a Get request. This request URL will return a JSON object which will be deserialized to an object of type <span style="color:yellow;background-color:black;">AvailableLanguage</span>.

## Configuring the Service

To make the service available to the components we need to configure it on the server-side app. Open the <span style="color:yellow;background-color:black;">Startup.cs file</span>. Add the following line inside the <span style="color:yellow;background-color:black;">ConfigureServices</span> method of Startup class.

```code

    services.AddSingleton<TranslationService>();

```

## Creating the Blazor UI Component

We will add the Razor page in the <span style="color:yellow;background-color:black;">BlazorTranslator/Pages</span> folder. By default, we have “Counter” and “Fetch Data” pages provided in our application. These default pages will not affect our application but for the sake of this tutorial, we will delete fetchdata and counter pages from <span style="color:yellow;background-color:black;">BlazorTranslator/Pages</span> folder.

Right-click on the <span style="color:yellow;background-color:black;">BlazorTranslator/Pages folder</span> and then select Add >> New Item. An “Add New Item” dialog box will open, select “Visual C#” from the left panel, then select “Razor Component” from the templates panel, put the name as <span style="color:yellow;background-color:black;">Translator.razor</span>. Click Add. Refer to the image shown below.

![image info](/img/azure/11/AddBlazorComponet.png)

We will add a code-behind file for this razor page to keep the code and presentation separate. This will allow easy maintenance for the application.  Right-click on the <span style="color:yellow;background-color:black;">BlazorComputerVision/Pages</span> folder and then select Add >> Class. Name the class as <span style="color:yellow;background-color:black;">OCR.razor.cs</span>. The Blazor framework is smart enough to tag this class file to the razor file. Refer to the image shown below.

![image info](/img/azure/10/OCRStuct.png)

Open the Translator.razor file and add the following code at the top.

```code

    @page "/translator"
    @using BlazorTranslator.Models
    @using BlazorTranslator.Data
    @inject TranslationService translationService

```

We have defined the route for this component. We are also injecting the <span style="color:yellow;background-color:black;">TranslationService</span> in this component.

Now we will add the following HTML code in this file.

```code

    <h3>Multilanguage translator using Microsoft Translator API Cognitive Service</h3>
<hr />
<div class="container">
    <div class="row">
        <div class="col-md-6">
            <select class="form-control" @bind="inputLanguage">
                <option value="">-- Select input language --</option>
                @foreach (KeyValuePair<string, LanguageDetails> lang in LanguageList)
                {
                    <option value="@lang.Key">@lang.Value.Name</option>
                }
            </select>
            <textarea placeholder="Enter text to translate" class="form-control translation-box" rows="5" @bind="@inputText"></textarea>
        </div>
        <div class="col-md-6">
            <select class="form-control" @onchange="SelectLanguage">
                <option value="">-- Select target language --</option>
                @foreach (KeyValuePair<string, LanguageDetails> lang in LanguageList)
                {
                    <option value="@lang.Key">@lang.Value.Name</option>
                }
            </select>
            <textarea disabled class="form-control translation-box" rows="5">@outputText</textarea>
        </div>
    </div>
    <div class="text-center">
        <button class="btn btn-primary btn-lg" @onclick="Translate">Translate</button>
    </div>
</div>
```
We have defined two dropdown lists one each for input language and the target language. The Azure Translate Text API will detect the input language and we will use that value to populate the dropdown for input language. We have also defined two text areas for the input and the translated text.

Finally, add the following code in the <span style="color:yellow;background-color:black;">@code </span> section of the page.

```code
    @code {
        private TranslationResult[] translations;
        private AvailableLanguage availableLanguages;
        private string outputLanguage { get; set; }
        private string inputLanguage { get; set; }
        string inputText { get; set; }
        string outputText { get; set; }
        private Dictionary<string, LanguageDetails>
        LanguageList = new Dictionary<string, LanguageDetails>();

        protected override async Task OnInitializedAsync()
        {
            availableLanguages = await translationService.GetAvailableLanguages();
            LanguageList = availableLanguages.Translation;
        }
        private void SelectLanguage(ChangeEventArgs langEvent)
        {
            this.outputLanguage = langEvent.Value.ToString();
        }
        private async Task Translate()
        {
            if (!string.IsNullOrEmpty(outputLanguage))
            {
                translations = await translationService.GetTranslatation(this.inputText, this.outputLanguage);
                outputText = translations[0].Translations[0].Text;
                inputLanguage = translations[0].DetectedLanguage.Language;
            }
        }
    }
```

We are invoking the <span style="color:yellow;background-color:black;">GetAvailableLanguages</span> method from our service inside the <span style="color:yellow;background-color:black;">OnInitializedAsync</span>. This <span style="color:yellow;background-color:black;">OnInitializedAsync</span> is a lifecycle method that will be invoked upon component initialization. This will make sure that the language dropdown will be populated as the page loads.

The <span style="color:yellow;background-color:black;">SelectLanguage</span> method will be set the <span style="color:yellow;background-color:black;">outputLanguage</span> for the translation. The Translate method will invoke the <span style="color:yellow;background-color:black;">GetTranslatation</span> method from the service. We will set the <span style="color:yellow;background-color:black;">outputText</span> and the language detected for the <span style="color:yellow;background-color:black;">inputLanguage</span> as returned from the service.

## Add styling for the Translator component
Navigate to BlazorTranslator\wwwroot\css\site.css file and put the following style definition inside it.

```code

    .translation-box {
        margin: 15px 0px;
    }
```

## Adding Link to Navigation menu
The last step is to add the link of our Translator component in the navigation menu. Open BlazorTranslator/Shared/NavMenu.razor file and add the following code into it.

```code 

    <li class="nav-item px-3">
    <NavLink class="nav-link" href="translator">
        <span class="oi oi-list-rich" aria-hidden="true"></span> Translator
    </NavLink>
    </li>
```
Remove the navigation links for Counter and Fetch-data components as they are not required for this application.

## Execution Demo

Press F5 to launch the application. Click on the Translator button on the nav menu in the left. You can perform the multilanguage translation as shown in the image below.

![image info](/img/azure/11/output.gif)

## Summary

We have created a Translator Text Cognitive Services resource on Azure. We have used the Translator Text API to create a multilanguage translator using Blazor. This translator supports more than 60 languages for translation. We fetched the list of supported languages for translation from the global API endpoint for Translator Text.

Get the Source code from [GitHub](https://github.com/girishgodage/Blazor-Translator-Azure-Cognitive-Services) and play around to get a better understanding.