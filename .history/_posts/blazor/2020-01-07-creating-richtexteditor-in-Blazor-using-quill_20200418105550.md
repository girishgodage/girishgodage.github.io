---
title: Creating A Rich Text Editor In Blazor Using Quill
date: 2020-01-07 10:41:00 Z
permalink: "/blog/creating-richtexteditor-in-Blazor-using-quill"
categories:
- Blazor
tags:
- learning
summary: In this article, using Quill in Blazor, we will demonstrate how to  Create A Rich Text Editor
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-creating-richtexteditor-in-Blazor-using-quill
---

## Creating A Rich Text Editor In Blazor Using Quill

 In this article, using Quill in Blazor, we will demonstrate how to  Create A Rich Text Editor

![image info](/img/blazor/2/Blazoreditor.gif)

You can easily implement a **Rich Text Editor** in your **Blazor** applications using the free open source component called [Quill](https://quilljs.com/).

## Setup

Create a new **Server Side Blazor** project and *right-click* on the **wwwroot** folder and select **Add** then **New Item…**

![image info](/img/blazor/2/1.png)

Create a **JavaScript** file called **BlazorQuill.js**.

![image info](/img/blazor/2/2.png)

For the file, use the following content:

```
(function () {
    window.QuillFunctions = {        
        createQuill: function (quillElement) {
            var options = {
                debug: 'info',
                modules: {
                    toolbar: '#toolbar'
                },
                placeholder: 'Compose an epic...',
                readOnly: false,
                theme: 'snow'
            };
            // set quill at the object we can call
            // methods on later
            new Quill(quillElement, options);
        }
    };
})();

```

![image info](/img/blazor/2/3.png)

Open the **_Host.cshtml** file and add the following **.css** references to the **Head** section (above the **bootstrap .css** lines):

```

 <!-- Quill stylesheet -->
    <link href="//cdn.quilljs.com/1.3.6/quill.snow.css" rel="stylesheet">
    <link href="//cdn.quilljs.com/1.3.6/quill.bubble.css" rel="stylesheet">

```

Next, add the following lines to the **Body** section (below the **blazor.server.js** reference):

```

<!-- Quill library -->
    <script src="https://cdn.quilljs.com/1.3.6/quill.js"></script>
    <script src="BlazorQuill.js"></script>

```

![image info](/img/blazor/2/4.png)

Open the **Index.razor** file and change *all* the content to the following:

```

@page "/"
@*Inject JSRuntime to allow JavaScript Interop *@
@inject IJSRuntime JSRuntime
@if (EditorEnabled)
{
    <div id="toolbar">
        <span class="ql-formats">
            <select class="ql-font">
                <option selected=""></option>
                <option value="serif"></option>
                <option value="monospace"></option>
            </select>
            <select class="ql-size">
                <option value="small"></option>
                <option selected=""></option>
                <option value="large"></option>
                <option value="huge"></option>
            </select>
        </span>
        <span class="ql-formats">
            <button class="ql-bold"></button>
            <button class="ql-italic"></button>
            <button class="ql-underline"></button>
            <button class="ql-strike"></button>
        </span>
        <span class="ql-formats">
            <select class="ql-color"></select>
            <select class="ql-background"></select>
        </span>
        <span class="ql-formats">
            <button class="ql-list" value="ordered"></button>
            <button class="ql-list" value="bullet"></button>
            <button class="ql-indent" value="-1"></button>
            <button class="ql-indent" value="+1"></button>
            <select class="ql-align">
                <option selected=""></option>
                <option value="center"></option>
                <option value="right"></option>
                <option value="justify"></option>
            </select>
        </span>
        <span class="ql-formats">
            <button class="ql-link"></button>
        </span>
    </div>
}
<div @ref="@divEditorElement" />
@code {
    private string strSavedContent = "";
    private ElementReference divEditorElement;
    private string EditorContent;
    private string EditorHTMLContent;
    private bool EditorEnabled = true;
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await JSRuntime.InvokeAsync<string>(
                "QuillFunctions.createQuill", divEditorElement);
        }
    }   
}

```

![image info](/img/blazor/2/5.png)

Hit **F5** to *run* the application.

![image info](/img/blazor/2/6.png)

The **Quill Rich Text Editor** will appear.

## Programmatically Get Content

The Quill control provides the content as **Text**, **HTML**, or its native [Delta format](https://quilljs.com/docs/delta/).

Add the following buttons to the markup section in the **Index.razor** page:

``` 

<br />
<button class="btn btn-primary" @onclick="GetText">Get Text</button>
<button class="btn btn-primary" @onclick="GetHTML">Get HTML</button>
<button class="btn btn-primary" @onclick="GetEditorContent">Get Content</button>
<br />
 
```
Also add some Divs to display the content: