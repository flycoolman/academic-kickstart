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
lastmod: "2020-09-06T00:00:00Z"
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

#### Import the Project
Follow the steps below to import the project.  

Import projects... or File ---> Import...
![import-projects](./import-projects.png)  
![existing-maven-projects](./existing-maven-projects.png)  
![select-all-pom-files](./select-all-pom-files.png)  

{{% alert note %}}
The import is done by m2e plugin.  
The **warning** shown below can be ignored.
maven-remote-resources-plugin (goal "process") is ignored by m2e.
{{% /alert %}}


#### Build and Test

Build and test as separate steps, i.e.

- Run As ---> Maven clean
- Run As ---> Maven build
- Run As ---> Maven test
- Run As ---> Maven install

![maven-run-as-options](./maven-run-as-options.png)  

Or define the goals at one time, i.e.
Run As ---> Maven build...  ---> Goals: (clean install)
![maven-custom-build](./maven-custom-build.png)  


{{% alert note %}}
Specific module can be chosen and do the same build.  
Maven will build the dependencies automatically.
{{% /alert %}}

#### Issues and Tricks

- Too many files with unapproved license
When doing 'install', the below error occurs. No issue with 'build' and 'test', but with 'install'

>[INFO] BUILD FAILURE
> Too many files with unapproved license

**Solution**  
Use or check out clean source code, then do 'install'.

- Build failed with 8 threads
When setting 8 threads for build, the build failed.

**Solution**  
Set build threads as 1.

![1-thread](./1-thread.png)

<br>

### IntelliJ IDEA

Inversion of Control is too generic a term, and thus people find it confusing. As a result with a lot of discussion with various IoC advocates, the name was settled on **Dependency Injection**.  
Dependency injection generally means passing a dependent object as a parameter to a method, rather than having the method create the dependent object.
What it means in practice is that the method does not have a direct dependency on a particular implementation; any implementation that meets the requirements can be passed as a parameter.

{{% alert note %}}
The Spring IoC Container is the leading dependency injection framework. 
{{% /alert %}}


#### Import the Project
Follow the steps below to import the project.  

Import projects... or File ---> Import...
![import-projects](./import-projects.png)  
![existing-maven-projects](./existing-maven-projects.png)  
![select-all-pom-files](./select-all-pom-files.png)  

{{% alert note %}}
The **warning** shown below can be ignored.
maven-remote-resources-plugin (goal "process") is ignored by m2e.
{{% /alert %}}


#### Build and Test

Build and test as separate steps, i.e.

- Run As ---> Maven clean
- Run As ---> Maven build
- Run As ---> Maven test
- Run As ---> Maven install

![maven-run-as-options](./maven-run-as-options.png)  

Or define the goals at one time, i.e.
Run As ---> Maven build...  ---> Goals: (clean install)
![maven-custom-build](./maven-custom-build.png)  


{{% alert note %}}
Specific module can be chosen and do the same build.  
Maven will build the dependencies automatically.
{{% /alert %}}

#### Issues and Tricks

- Too many files with unapproved license
When doing 'install', the below error occurs. No issue with 'build' and 'test', but with 'install'

>[INFO] BUILD FAILURE
> Too many files with unapproved license

**Solution**  
Use or check out clean source code, then do 'install'.



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
