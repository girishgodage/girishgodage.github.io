---
title: Creating A Step-By-Step End-To-End Database Server-Side Blazor Application
date: 2020-03-10 10:26:00 Z
permalink: "/blog/creating-step-by-step-ServerSideBlazorApp"
categories:
- Blazor
tags:
- learning
summary: In this article, we will demonstrate how a list of Weather forecasts can be added to the database by each user. A user will only have the ability to see their own forecasts.
image: "/img/aws_cloud.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2020-01-07-creating-creating-step-by-step-ServerSideBlazorApp
---

## Creating A Step-By-Step End-To-End Database Server-Side Blazor Application

The primary benefit we have when using **server-side Blazor** is that we do not have to make web http calls from the client code to the server code. This reduces the code we need to write and eliminates many security concerns.

In this article, we will demonstrate how a list of **Weather forecasts** can be added to the database by each user. A user will only have the ability to see their own forecasts.

![image info](/img/blazor/3/1.png)

## Use SQL Server

![image info](/img/blazor/3/2.png)

The new project template in **Visual Studio** will allow you to create a database using [SQL Server Express LocalDB](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/working-with-sql?view=aspnetcore-2.2&tabs=visual-studio#sql-server-express-localdb). However, it can be problematic to install and configure. Using the free **SQL Server 2019 Developer server** (or the full **SQL Server**) is recommended.

Download and install **SQL Server 2019 Developer Edition** from the following link:

https://www.microsoft.com/en-us/sql-server/sql-server-downloads



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
Also add some **Divs** to display the content:

```

<br />
<div>
    @EditorContent
</div>
<div>
    @((MarkupString)@EditorHTMLContent)
</div>

``` 

In the **@code** section implement the methods:

```
    async Task GetText()
    {
        EditorHTMLContent = "";
        EditorContent = await JSRuntime.InvokeAsync<string>(
            "QuillFunctions.getQuillText", divEditorElement);
    }
    async Task GetHTML()
    {
        EditorContent = "";
        EditorHTMLContent = await JSRuntime.InvokeAsync<string>(
            "QuillFunctions.getQuillHTML", divEditorElement);
    }
    async Task GetEditorContent()
    {
        EditorHTMLContent = "";
        EditorContent = await JSRuntime.InvokeAsync<string>(
            "QuillFunctions.getQuillContent", divEditorElement);
    }

```

Finally, in the **BlazorQuill.js** file, add the following code under the **createQuill** method:

```
,
        getQuillContent: function (quillControl) {
            return JSON.stringify(quillControl.__quill.getContents());
        },
        getQuillText: function (quillControl) {
            return quillControl.__quill.getText();
        },
        getQuillHTML: function (quillControl) {
            return quillControl.__quill.root.innerHTML;
        }
```

![image info](/img/blazor/2/7.png)

When we run the application, we can enter **rich text** in the **editor**, and *programmatically* display it.

## Saving And Loading Content

If we want to *save and load* content into the **Quill** editor, for example, to persist to a database, we can implement it in the following way.

Add the following buttons:

``` 

<br />
<button class="btn btn-danger" @onclick="SaveContent">Save Content</button>
<button class="btn btn-success" @onclick="LoadContent">Load Saved Content</button>
<br />
 
```

Add the methods to implement the buttons:


``` 

    async Task SaveContent()
    {
        strSavedContent = await JSRuntime.InvokeAsync<string>(
            "QuillFunctions.getQuillContent", divEditorElement);
    }
    async Task LoadContent()
    {
        var QuillDelta = await JSRuntime.InvokeAsync<object>(
            "QuillFunctions.loadQuillContent", divEditorElement, strSavedContent);
    }
 
```

Finally, in the **BlazorQuill.js** file, add the following code under the **getQuillHTML** method:

``` 

        ,
        loadQuillContent: function (quillControl, quillContent) {
            content = JSON.parse(quillContent);
            return quillControl.__quill.setContents(content, 'api');
        },
```

![image info](/img/blazor/2/8.png)

When we run the application, we can enter text, and click the **Save Content** button.

![image info](/img/blazor/2/9.png)

*Clear the content and enter new content…*

![image info](/img/blazor/2/10.png)

Click Load Saved Content, and have the original content come back.

## Disable The Editor

While it is possible to load and save **Text** or **HTML** content from the control and display that content on any page, to display content in the **Quill** editor native Delta format, you have to load that content into the **Quill** editor control.

When you want the content to be read only, you will want to disable the editor. You can do that programmatically by adding a button:

 
```
<br />
<button class="btn btn-info" @onclick="DisableQuillEditor">Disable Editor</button>
<br />
 
```
A method to implement the button:

 ```
    async Task DisableQuillEditor()
    {
        EditorEnabled = false;
        await JSRuntime.InvokeAsync<object>(
            "QuillFunctions.disableQuillEditor", divEditorElement);
    }
 
```
…and a method in the **BlazorQuill.js** file:

``` 

        ,
        disableQuillEditor: function (quillControl) {
            quillControl.__quill.enable(false);
        }

```

![image info](/img/blazor/2/11.png)

Clicking the **Disable Editor** button will put the editor in read only mode.