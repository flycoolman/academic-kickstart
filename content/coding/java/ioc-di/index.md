---
title: 'Inversion of Control and Dependency Injection'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A summary of Spring Inversion of Control and Dependency Injection.
authors:
- admin
tags:
- Java
- Spring
- IoC
- DI
categories:
- Java
date: "2020-08-01T00:00:00Z"
lastmod: "2020-08-30T00:00:00Z"
featured: false
draft: false

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

### Inversion of Control (IoC)

A real system might have dozens of services and components. To make a loosely coupled application, the way is to plug in the plugins (components and services) at some point. 
So the core problem is how to assemble the plugins into an application. Then frameworks aim to resolve the problem. Usually **Inversion of Control** is used in frameworks, so does Spring Framework. That's why Inversion of Control (IoC) is the core technology of Spring Framework.

#### Inversion of Control (IoC) vs. Traditional Control

- Traditional Control
  In traditional programming, the custom code that expresses the purpose of the program calls into reusable libraries to take care of generic tasks.  
  I.e.   
  the custom object instantiates its dependent objects, then uses the objects.

- Inversion of Control (IoC)
  IoC inverts the flow of control as compared to traditional control flow. In IoC, custom-written portions of a computer program receive the flow of control from a generic framework.
  Usually it is the framework that calls into the custom, or task-specific, code.  
  I.e.  
  the custom object receives the instantiated dependent objects from framework.

#### What Can IoC Serve

IoC as a design guideline, is used to increase modularity of the program and make it extensible. It serves the following purposes:

- To decouple the execution of a task from implementation.
- To make every module focus on what it is designed for.
- To free modules from assumptions about how and what other systems do, and instead rely on contracts.
- To prevent side effects on other modules when replacing a module.


#### Spring Implementation of IoC Principle

**IoC** is also known as **dependency injection (DI)**. It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.

This is common characteristic of frameworks, IoC manages java objects:
- from instantiation to destruction through its BeanFactory.
- Java components that are instantiated by the IoC container are called beans, and the IoC container manages a bean's scope, lifecycle events, and any AOP features for which it has been configured and coded. 

{{% alert note %}}
In Spring framework, the IoC Container does that job of injecting dependancies (DI) and not us, The flow of control is reversed, (Framework to Application) it is IoC with DI. 
{{% /alert %}}

<br>

### Spring IoC Container

Central to the Spring Framework is its inversion of control (IoC) container, which provides a consistent means of configuring and managing Java objects using reflection. The container is responsible for managing object lifecycles of specific objects: creating these objects, calling their initialization methods, and configuring these objects by wiring them together.

The interface **org.springframework.context.ApplicationContext** represents the Spring IoC container and is responsible for **instantiating**, **configuring**, and **assembling** the aforementioned beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading **configuration metadata**. 

{{% alert note %}}
- Representation - org.springframework.context.ApplicationContext
- Responsibilities - instantiating, configuring, and assembling Beans
- Tool: configuration metadata
{{% /alert %}}


#### Types of IoC Containers

- BeanFactory container
- ApplicationContext container

<br>


### Dependency Injection (DI)

Inversion of Control is too generic a term, and thus people find it confusing. As a result with a lot of discussion with various IoC advocates, the name was settled on **Dependency Injection**.  
Dependency injection generally means passing a dependent object as a parameter to a method, rather than having the method create the dependent object.
What it means in practice is that the method does not have a direct dependency on a particular implementation; any implementation that meets the requirements can be passed as a parameter.

{{% alert note %}}
The Spring IoC Container is the leading dependency injection framework. 
{{% /alert %}}


#### Dependency Lookup vs. Dependency Injection

Objects can be obtained by means of either dependency lookup or dependency injection.  
- Dependency lookup is a pattern where a caller asks the container object for an object with a specific name or of a specific type. 
- Dependency injection is a pattern where the container passes objects by name to other objects, via either constructors, properties, or factory methods.


#### The Styles of DI

Dependency Injection can be done by:  
- Constructor-based dependency injection
  Constructor-based DI is accomplished by the container invoking a constructor with a number of arguments, each representing a dependency.

        public class SimpleMovieLister {

            // the SimpleMovieLister has a dependency on a MovieFinder
            private MovieFinder movieFinder;

            // a constructor so that the Spring container can inject a MovieFinder
            public SimpleMovieLister(MovieFinder movieFinder) {
                this.movieFinder = movieFinder;
            }

            // business logic that actually uses the injected MovieFinder is omitted...
        }

- Setter-based dependency injection
  Setter-based DI is accomplished by the container calling setter methods on your beans after invoking a no-argument constructor or a no-argument static factory method to instantiate your bean.

        public class SimpleMovieLister {

            // the SimpleMovieLister has a dependency on the MovieFinder
            private MovieFinder movieFinder;

            // a setter method so that the Spring container can inject a MovieFinder
            public void setMovieFinder(MovieFinder movieFinder) {
                this.movieFinder = movieFinder;
            }

            // business logic that actually uses the injected MovieFinder is omitted...
        }

#### Constructor-based or Setter-based DI

- Constructor-based and setter-based DI can be mixed
- Constructors for mandatory dependencies and setter methods or configuration methods for optional dependencies

**Why**
- Constructor injection lets you implement application components as immutable objects and ensures that required dependencies are not null. Furthermore, constructor-injected components are always returned to the client (calling) code in a fully initialized state. 
- Setter injection should primarily only be used for optional dependencies that can be assigned reasonable default values within the class. Otherwise, not-null checks must be performed everywhere the code uses the dependency. One benefit of setter injection is that setter methods make objects of that class amenable to reconfiguration or re-injection later.


{{% alert note %}}
Use the DI style that makes the most sense for a particular class.
{{% /alert %}}


### IoC vs. DI

- Interchangable
  IoC and DI are used interchangeably.
- Process and Result
  IoC is achieved through DI. DI is the process of providing the dependencies and IoC is the end result of DI.
- One to Many
  DI is a specific type of IoC and is not the only way to achieve IoC. There are other ways as well, such as:
    * Service Locator pattern
    * Template method design pattern
    * Strategy design pattern

By DI, the responsibility of creating objects is shifted from our application code to the Spring container; this phenomenon is called IoC.

DI is not the only way of removing the dependency from the application class to the plugin implementation. The other pattern you can use to do this is Service Locator, and I'll discuss that after I'm done with explaining Dependency Injection.


{{% alert note %}}
IoC basically facilitates having different components designed and coded separately and later used together by defining their relation with DI.
{{% /alert %}}




### Links
[The IoC Container](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-introduction)  
[Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html)  
[Spring Framework](https://en.wikipedia.org/wiki/Spring_Framework#Inversion_of_control_container_.28dependency_injection.29)  
[What is Dependency Injection and Inversion of Control in Spring Framework?](https://stackoverflow.com/questions/9403155/what-is-dependency-injection-and-inversion-of-control-in-spring-framework)  
[Spring – Inversion of Control vs Dependency Injection](https://howtodoinjava.com/spring-core/spring-ioc-vs-di/)  
[Spring – IoC Containers](https://howtodoinjava.com/spring-core/different-spring-ioc-containers/)  
[Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control#Implementation_techniques)  
[What is Dependency Injection and Inversion of Control in Spring Framework?](https://stackoverflow.com/questions/9403155/what-is-dependency-injection-and-inversion-of-control-in-spring-framework)  


<br>

#### Did you find this page helpful? Consider sharing it 🙌
