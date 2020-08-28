---
title: 'Spring Beans'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A summary of Spring Beans.
authors:
- admin
tags:
- Java
- Spring
categories:
- Java
- Spring
date: "2020-07-27T00:00:00Z"
lastmod: "2020-08-23T00:00:00Z"
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  caption: ''
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### Spring Beans

{{% alert note %}}
**The objects that are managed by Spring IoC* container**
{{% /alert %}}

The objects that form the backbone of your application and that are managed by the **Spring IoC container** are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. These beans are created with the **configuration metadata** that you supply to the container, for example, in the form of XML <bean/> definitions.

In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.

Spring Bean is nothing special, any object in the Spring framework that we initialize through Spring container is called Spring Bean. Any normal Java POJO class can be a Spring Bean if it’s configured to be initialized via container by providing configuration metadata information.


### Spring IoC Container

{{% alert note %}}
**org.springframework.context.ApplicationContext**
{{% /alert %}}

The interface org.springframework.context.ApplicationContext represents the Spring IoC container and is responsible for instantiating, configuring, and assembling the aforementioned beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. 


### Configuration Metadata

- This configuration metadata represents how you as an application developer tell the Spring container to instantiate, configure, and assemble the objects in your application.
- The configuration metadata is represented in **XML**, **Java annotations**, or **Java code**. 
- It allows you to express the objects that compose your application and the rich interdependencies between such objects.
- Spring configuration consists of at least one and typically more than one bean definition that the container must manage. 
- Consumed by Spring IoC container


### Spring Bean Scopes
https://www.journaldev.com/2461/spring-ioc-bean-example-tutorial

### Spring Bean Lifecycle

### Spring Bean Instantiation

- Instantiation with a constructor
- Instantiation with a static factory method
- Instantiation using an instance factory method


#### Instantiation with a constructor

#### Instantiation with a static factory method

#### Instantiation using an instance factory method

### Spring Bean Configuration/Container Configuration
https://www.journaldev.com/2461/spring-ioc-bean-example-tutorial

#### XML-based

XML-based configuration metadata shows these beans configured as <bean/> elements inside a top-level <beans/> element.

#### Annotation-based

Annotation injection is performed before XML injection, thus the latter configuration will override the former for properties wired through both approaches.



#### Java-based (JavaConfig)

Java configuration typically uses @Bean annotated methods within a @Configuration class.
Spring JavaConfig is a product of the Spring community that provides a pure-Java approach to configuring the Spring IoC Container. While JavaConfig aims to be a feature-complete option for configuration, it can be (and often is) used in conjunction with the more well-known XML-based configuration approach.

https://docs.spring.io/spring-javaconfig/docs/1.0.0.m3/reference/html/overview.html


### Java-based vs Annotation-based configuration





### Links
[JavaBeans](https://en.wikipedia.org/wiki/JavaBeans)
[Oracle's JavaBeans tutorials](http://download.oracle.com/javase/tutorial/javabeans/)
[JavaBeans specification](http://www.oracle.com/technetwork/java/javase/documentation/spec-136004.html)
[初识Spring —— Bean的装配（一）](https://juejin.im/post/6844903618567471112)  
[初识Spring —— Bean的装配（二）](https://juejin.im/post/6844903619834150919)  
[What in the world are Spring beans?](https://stackoverflow.com/questions/17193365/what-in-the-world-are-spring-beans)  
[Spring注入Bean的几种方式](https://juejin.im/post/6844903813753602056)  
[Java-based vs annotation-based configuration/autowiring](https://stackoverflow.com/questions/41615041/java-based-vs-annotation-based-configuration-autowiring)  
[Java-based vs annotation-based configuration/autowiring - spring](https://html.developreference.com/article/14966783/Java-based+vs+annotation-based+configuration+autowiring)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌
