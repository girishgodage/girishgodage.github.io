---
title: Introduction to Four Principles Of Simple Design
date: 2019-10-14 10:41:00 Z
categories:
- SoftwareDesign
tags:
- learning
summary: With agile and extreme programming, the focus is on keeping your design simple. How do you keep your design simple? How do you decide whether your code is good enough?
image: /img/sw_design.png
author: Girish Godage
layout: posts
prevurl: /blog/four-principles-of-simple-design
nexturl: /blog/software-design-seperation-of-concerns-with-examples
discussion_id: 2019-10-15-SwDesign1
permalink:  /blog/four-principles-of-simple-design
---

### What is Simple Design?

 ** With agile and extreme programming, the focus is on keeping your design simple. How do you       keep your design simple? How do you decide whether your code is good enough? **

It is very important to keep the design of your application simple.

In almost all agile projects, the aim is to meet today's requirements, with clean code.

> You go for complex design, only when simple design does not solve your problem.

### The Four Principles Of Simple Design

A software application is said to have a simple design if it:

* **Runs all tests**
* **Contains no duplication**
* **Expresses intent of programmers**
* **Minimizes number of classes and methods**

Let's now look at these aspects a little closely, by turn.

#### Runs All Tests

We want to keep running all the tests continuously, because we want the code to work, at all times.

An important corollary of this principle is you need to have a large number of automation tests. All unit, integration and API tests must be automated.

You should launch these tests as part of your build, and they should also be a part of Continuous Integration (CI).

With CI, you commit code into the repository, all the tests are run, and immediate feedback is there for you to act on.

Since the software is being tested all the time, it is stable.

#### Contains No Duplication

The second principle stresses on the fact that your code should have as little duplication as possible.

A good example is to create common components, whereever possible, in the design of large applications. This helps centralize the logic and allow other applications to reuse them.

Why do we hate duplication? 

If there is a need for a change, the same change needs to repeated at all these locations. The result : More effort and also possibilities of more defects when you miss making the change in every location. That is a sign of bad design.

#### Expresses Intent Of Programmers

Your code should be easy to read, and your design, simple to understand. This principle is also called **Clarity Of Code**.

Have a look at the following piece of code:

##### Example-01 v1

![image info](/img/sw_design/1/Capture-09-01.png)

 

Do you understand what it does? 

Now look at the following version of the same program: 

##### Example-01 v2

![image info](/img/sw_design/1/Capture-09-02.png)

Do you understand what it's trying to do?

Actually, Example-01 v2 results from applying the Four Principles of Simple Design to Example-01 v1.

Start with creating good names for variables, methods and classes. That improves clarity.

#### Minimize number of classes and methods

You should have 

* Small methods
* Small classes
* Minimum number classes and methods

Isn't it simple.

Lesser code you have, lesser code you have to maintain.

Always aim to keep things simple.


### Summary

In this article, we looked at the four principles of simple design. These are the first steps to ensure that the design of your application remains simple. It lays the foundation for applying more advanced principles to improving your design.

---
{% include swPrincipal.md %}


{% include blogslide.html %}

