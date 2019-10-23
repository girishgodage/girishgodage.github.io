---
title: Microservices Architectures - Centralized Configuration and Config Server
date: 2019-10-19 20:17:00 Z
permalink: "/blog/introduction-to-centralized-configuration-with-spring-cloud-config-server"
categories:
- SpringBootMicroservices
tags:
- learning
summary: In this article, we explore the concept of centralized configuration, in
  the context of microservices.
image: "/img/microservices.png"
author: Girish Godage
layout: posts
prevurl: "/blog/service-discovery-in-microservices"
nexturl: "/blog/introduction-to-api-gateways-with-microservices"
discussion_id: 2019-10-19-Microservices13
---

In this article, we explore the concept of centralized configuration, in the context of microservices. 

### You will learn
- What is a Cloud config server?
- What is centralized configuration?
- Why do we need centralized configuration in microservices architectures?
- What are the important features of Spring Cloud Config Server?

### Why Centralized Configuration?

When we talk about microservices architecture, we visualize a large number of small microservices talking to each other. The number of microservices depends on the size of the enterprise. 

[![image info](/images/Capture-058-03.png)

The  interesting part is that each of these microservices can have its own configuration. 

Such configuration includes details such as:
* Application configuration
* Database configuration
* Communication Channel Configuration - queues and other infrastructure
* URLs of other microservices to talk to

In addition, each microservice would have a separate configuration for different environments, such as development, qa and production. 

> If maintaining a single configuration for a large application is difficult, imagine maintaining configurations for hundreds of microservice in different environments

### Centralized Config Server To The Rescue

That's where a centralized configuration server steps in. 

Configuration for all microservices (for all environments) is stored at one place - a centralized configuration store. 

When a microservice needs its configuration, it provides an id at launch - a combination of microservice name and the environment. 

The centralize config server looks up the configuration and provides the configuration to the microservice.

> Ensure that the configuration in centralized config server is secured and has role based access.

### Introducing Spring Cloud Config Server

Spring Cloud Config Server is one of the popular implementations of a cloud config server.

Spring Cloud Config Server enables you to store all the configuration for multiple microservices for different environments in a Git or a SVN Repository. A set of folder structures and conventions need to be followed for the setup to work.

![image info](/images/Capture-058-02.png)

A microservice can connect to the config server and identify itself, and also specify the instance it represents. This enables it to get the required configuration. 

The setup ensures that the operations team does not need to take time out to configure the individual microservices on a case-by-case basis. All that they need to worry about is to configure the centralized config server, and start putting relevant configuration into the Git repository.

#### Automatically picking up configuration changes

An interesting feature present with the Spring Cloud Config Server, is auto refresh. 

Whenever a change is committed to the Git repository, configuration in the application is auto-refreshed.

### Summary

In this article, we looked at why we need centralized configuration in microservices based applications. We looked at how the Spring Cloud Config Server manages centralized configuration.

---
{% include microservices3.md %}

{% include blogslide.html %}

