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