---
title: How to build an eCommerce app using ReactJS
date: 2019-10-15 10:41:00 Z
permalink: "/blog//build-ecommerce-app-reactjs"
categories:
- Web Development
tags:
- learning
summary: Why use React for eCommerce" is the one of the biggest question clients ask to a developer. Built by Facebook, ReactJS is one of the largely used UI Library that helps with the creation of beautiful web applications requiring minimal effort and coding. Things like Atomic design principles, component-driven approach, high-speed etc, make React js a popular choice of framework for building eCommerce web applications.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: ''
nexturl: ''
discussion_id: 2019-10-20-build-ecommerce-app-reactjs
---

## How to build an eCommerce app using ReactJS

 "Why use React for eCommerce" is the one of the biggest question clients ask to a developer. Built by Facebook, ReactJS is one of the largely used UI Library that helps with the creation of beautiful web applications requiring minimal effort and coding. Things like Atomic design principles, component-driven approach, high-speed etc, make React js a popular choice of framework for building eCommerce web applications.

 ![image info](/img/webdevelopment/3/Ecommerce-app-using-Reactjs.png)

 A few weeks ago, when I wrote about **React performance**, I was asked by a lot of you to share my thoughts on how to build a performant interface for eCommerce app using ReactJS. And, as I wrote email responses, this blog post started taking its shape.

We don't just fine tune eCommerce apps for no reason. The fact that your product team has to struggle and match various metrics with conversion and performance itself shows how even the tiniest bit of UI performance gain reflects into a significant revenue boost.

What you can expect to learn here:

* The key points which makes React best fit for an eCommerce site's frontend
* The practical use case of using Redux for eCommerce
* The best practice of architecting an eCommerce app with Redux

Just to give you a better overview, this is what our eCommerce app with ReactJS will look like:

![image info](/img/webdevelopment/3/Ecommerce-app-with-React.jpg)


## Building eCommerce app with ReactJS – Things to Consider

Expansion is the goal of any eCommerce organisation. And why not? Your business wasn't built to be small.

The frontend of an eCommerce app acts as an indicator of the quality of your products and overall brand value. But above all else, it provides a certain level of trust and comfort to the user.

Do you know that 

> "About 97% of an eCommerce performance optimisation is only Front-end related, leaving beside only 3% for back-end."?

This is how insanely impactful an eCommerce front end really is!

![image info](/img/webdevelopment/3/Ecommerce-performance-optimization.gif)

Moreover, an eCommerce frontend might gets often updated for even minor changes (like changing a font) on the special days of sale such as good-Friday which requires deploying the update to the entire system.

Henceforth, frontend performance optimization needs to be handled with care otherwise you might end up breaking your app which would be otherwise expensive for your business.

So, how does we handle all frontend-related optimization in eCommerce?

Well, the answer is, USE React...

While modern web has Angular, Vue and other frameworks to help support building complex UIs, with the exception of Vue, we face issues with unnecessary complexities with them.

## Benefits of using Reactjs for eCommerce frontend
I've been working extensively in eCommerce segment for over a year and a half. In that crucial time, I've had experienced my fair-share of architectural mistakes, poor design-related decisions, technical debt and learned many things or two.

One of the greatest things I've learned in these on & half years is that React really wants you to follow KISS (Keep it Simple, Stupid) and Atomic design principles.

I won't delve upon KISS principle assuming that you will understand it through the link itself. Instead, let's first understand the Atomic design principles for building complex user-interfaces.

### React : How Atomic Design principle helps in building eCommerce app?

According to the Brad Frost,

> "Atomic design is methodology for creating design systems backed with five 'building blocks', which, when combined, create semantic, contextual relationships between interface objects."

There are five building blocks in atomic design:

* **Atoms**: Everything in Atomic design begins with the smallest elements of the interface: the atoms. These are small, contained and simplest components of an application.
  
* **Molecules**: Molecules are groups of two or more atoms held together by chemical bonds.
  
* **Organisms**: Organisms are assemblies of molecules functioning together as a unit.
  
* **Templates**: Templates consist mostly of groups of organisms stitched together to form pages.

* **Pages**: Pages are specific instances of templates.
  

![image info](/img/webdevelopment/3/Atomic-design-layers.png)

### Applying Atomic design principles to eCommerce
Now that we have understood what an Atomic design guidelines is, Let's explore how to apply it while building a complex interface such as eCommerce.

To start with Atoms, let's evaluate which components will act as atoms in eCommerce?

### Atoms in ReactJS for eCommerce
Since Atoms are the smallest individual components of a webpage or an app. They can be anything such as a TextInput component or a Button component.

### Molecule in ReactJS for eCommerce
Now, let's say that we want to make a search module for eCommerce with a Button component and a TextInput component. When we combine these two components, we can create a Search component that acts as a molecule.

![image info](/img/webdevelopment/3/Search-2.png)

### Organism in ReactJS for eCommerce
Organisms are most likely the complex UI elements of our eCommerce app. Since they're more complex than molecules and atoms, it's possible that the organisms in our applications need to handle business logic.

Let's say, or Product page in eCommerce is made up of organisms such as a Header organism and a ProductGrid organism that contains Product molecules. Now, this is how our Header organism looks like:

![image info](/img/webdevelopment/3/Ecommerce-header.png)

### Template in ReactJS for eCommerce
Templates are the combination of atoms, molecules and organisms altogether held in several containers. It is the combination of these smaller components which makes up user interfaces in our applications.

By combining all such atoms, molecules and organisms, we can finally build our Template which will be wireframe for our eCommerce app.

![image info](/img/webdevelopment/3/Ecommerce-header.png)