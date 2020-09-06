---
title: 'Set Up Shardingsphere Development Environment'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: This documents wrote down the experience of setting up development environment for Shardingsphere.
authors:
- admin
tags:
- Java
- Shardingsphere
- Database
categories:
- Java
- Shardingsphere
- Database
date: "2020-08-01T00:00:00Z"
lastmod: "2020-08-30T00:00:00Z"
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

## Set Up Shardingsphere Development Environment

This document used the official release version to set up and verify development environment. 
This way could help to rule out any unstable issues of source code and to focus the issues on environment.

## Prerequisites

- Linux (Ubuntu 18.04)
- Source code [4.1.1](https://www.apache.org/dyn/closer.cgi/shardingsphere/4.1.1/apache-shardingsphere-4.1.1-src.zip) 
- Eclipse
- IntelliJ IDEA 

{{% alert note %}}
Choose the proper IDE (Eclipse or IntelliJ IDEA), even No IDE 
{{% /alert %}}

### Java Development Environment (No IDE)

#### Install JDK 8

    sudo apt install openjdk-8-jdk

    $ java -version
    openjdk version "1.8.0_252"
    OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1~18.04-b09)
    OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)

#### Install Maven (Optional)

    sudo apt install maven

    $ mvn -version
    Apache Maven 3.6.0
    Maven home: /usr/share/maven
    Java version: 1.8.0_252, vendor: Private Build, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
    Default locale: en_US, platform encoding: UTF-8
    OS name: "linux", version: "5.4.0-42-generic", arch: "amd64", family: "unix"

#### Unzip Source Code

- Download [Source Code](https://www.apache.org/dyn/closer.cgi/shardingsphere/4.1.1/apache-shardingsphere-4.1.1-src.zip)

- Unzip the source code

        unzip apache-shardingsphere-4.1.1-src.zip

- Change file permissions 

        chmod -R 755 apache-shardingsphere-4.1.1-src-release/

#### Build and Test

Based the Github page [Build Apache ShardingSphere](https://github.com/apache/shardingsphere), there is a script to do the build  

    ./mvnw clean install -Prelease

{{% alert note %}}
Make sure all the tests pass 
{{% /alert %}}

#### Issues and Tricks

- Multiple Java version installed


- Lombok in the project not support Java 11




<br>

### Eclipse

Central to the Spring Framework is its inversion of control (IoC) container, which provides a consistent means of configuring and managing Java objects using reflection. The container is responsible for managing object lifecycles of specific objects: creating these objects, calling their initialization methods, and configuring these objects by wiring them together.

The interface **org.springframework.context.ApplicationContext** represents the Spring IoC container and is responsible for **instantiating**, **configuring**, and **assembling** the aforementioned beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading **configuration metadata**. 

{{% alert note %}}
- Representation - org.springframework.context.ApplicationContext
- Responsibilities - instantiating, configuring, and assembling Beans
- Tool: configuration metadata
{{% /alert %}}


#### Types of IoC Containers


<Project Directory>/.metadata/.plugins/org.eclipse.m2e.core

lifecycle-mapping-metadata.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <lifecycleMappingMetadata>
        <pluginExecutions>
            <pluginExecution>
                <pluginExecutionFilter>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-remote-resources-plugin</artifactId>
                    <versionRange>[1.0,)</versionRange>
                    <goals>
                        <goal>process</goal>
                    </goals>
                </pluginExecutionFilter>
                <action>
                    <ignore>
                    </ignore>
                </action>
            </pluginExecution>
        </pluginExecutions>
    </lifecycleMappingMetadata>


Import projects...

File ---> Import...

Build and test as separate steps, i.e.

- Run As ---> Maven clean
- Run As ---> Maven build
- Run As ---> Maven test

Or define the goals at one time, i.e.
Run As ---> Maven build...  ---> Goals: (clean install)
![maven-build](./maven-build.png)  



maven-remote-resources-plugin (goal "process") is ignored by m2e.




{{% alert note %}}
- In short, the BeanFactory provides the configuration framework and basic functionality, and the ApplicationContext adds more enterprise-specific functionality. The ApplicationContext is a complete superset of the BeanFactory.  
- You should use an ApplicationContext unless you have a good reason for not doing so, with GenericApplicationContext and its subclass AnnotationConfigApplicationContext as the common implementations for custom bootstrapping.
{{% /alert %}}

<br>

### IntelliJ IDEA

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

{{% alert note %}}
- IoC basically facilitates having different components designed and coded separately and later used together by defining their relation with DI.  
- By DI, the responsibility of creating objects is shifted from our application code to the Spring container; this phenomenon is called IoC.
{{% /alert %}}

<br>

### Links
[Maven - Build Life Cycle](https://www.tutorialspoint.com/maven/maven_build_life_cycle.htm#:~:text=When%20Maven%20starts%20building%20a,are%20registered%20with%20each%20phase.&text=A%20goal%20represents%20a%20specific,and%20managing%20of%20a%20project.)  
[Introduction to the Build Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)  
[eclipse - Apache Camel:m2e忽略了maven-remote-resources-plugin(目标“进程”)](https://www.coder.work/article/6328115)  
[m2e-execution-not-covered](https://www.eclipse.org/m2e/documentation/m2e-execution-not-covered.html)  
[Apache Camel: maven-remote-resources-plugin (goal “process”) is ignored by m2e](https://stackoverflow.com/questions/23908837/apache-camel-maven-remote-resources-plugin-goal-process-is-ignored-by-m2e)  
[Spring – IoC Containers](https://howtodoinjava.com/spring-core/different-spring-ioc-containers/)  
[Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control#Implementation_techniques)  
[What is Dependency Injection and Inversion of Control in Spring Framework?](https://stackoverflow.com/questions/9403155/what-is-dependency-injection-and-inversion-of-control-in-spring-framework)  
[Interface BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)  
[Interface ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)  
[The BeanFactory](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-beanfactory)  



<br>

#### Did you find this page helpful? Consider sharing it 🙌
