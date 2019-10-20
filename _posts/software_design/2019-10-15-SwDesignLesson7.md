---
title: Software Design - How do you keep your design simple?
date: 2019-10-15 09:14:00 Z
permalink: "/blog/software-design-keep-your-design-simple"
categories:
- SoftwareDesign
tags:
- learning
summary: With extreme programming and agile, the focus is being lean and keeping your
  design simple. In this context, how do you ensure that your design remains good
  and evolves along with the application requirements? Here are a five tips you can
  focus on.
image: "/img/sw_design.png"
author: Girish Godage
layout: posts
prevurl: "/blog/software-design-keep-your-design-simple"
nexturl: "/blog/design-patterns-for-beginners-with-java-examples"
discussion_id: 2019-10-15-SwDesign7
---

---

With extreme programming and agile, the focus is being lean and keeping your design simple. In this context, how do you ensure that your design remains good and evolves along with the application requirements? Here are a five tips you can focus on.

#### The Four Principle Of Simple Design

These are the foundation for keeping your design simple. When you try to learn the ropes using Extreme Programming, you really need to focus on these principles. 

A software application is said to have a simple design if it:
* **Runs all tests**
* **Contains no duplication**
* **Expresses intent of programmers**
* **Minimizes number of classes and methods**

You can read more about it [here](/blog/four-principles-of-simple-design){:target='_blank'}

#### SOLID Principles

This represents a good aim to have when designing Object Object Software. The term SOLID is an acronym for:

* Single Responsibility Principle
* Open-Closed Principle
* Liskov Substitution Principle
* Interface Segregation Principle
* Dependency Inversion Principle

You can read more about it [here](/blog/software-design-solid-principles){:target='_blank'}

#### Appropriate Patterns

Be choosy about the design pattern you use. Do not fill up your design with patterns, just because they are available. Understand the context of your application and the context of the design pattern. Make sure they make the right match before implementing design pattern.

#### Simple Hand-drawn Diagrams

Simple, hand-drawn diagrams are quite sufficient to communicate the initial design to the stakeholders and your peers. Trying to produce stunning, intricate diagrams only leads to a wastage of precious time. 

Once the design is stabilized, you can work on more concrete diagrams.

#### Great Unit Tests

This is a very important requirement for a simple design. Tests help to keep your design evolving, because they give you feed back on how correct your code is. If that is not the case, you won;t be confident of changing your design, and  the design would no longer evolve.

### Summary

In this article, we discussed a few tips on simple design.


---

{% include swPrincipal1.md %}

{% include blogslide.html %}

