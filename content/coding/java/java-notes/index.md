---
title: 'Java Notes'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A free style notes of Java.
authors:
- admin
tags:
- Java
categories:
- Java
date: "2020-07-17T00:00:00Z"
lastmod: "2020-08-22T00:00:00Z"
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

## Java Notes


{{% alert note %}}
- a Java classs 
- serializable
- zero-argument constructor
- getter and setter
{{% /alert %}}


### Differentiate JVM JRE JDK JIT

- Java Virtual Machine (JVM) is an abstract computing machine.
- Java Runtime Environment (JRE) is an implementation of the JVM.
- Java Development Kit (JDK) contains JRE along with various development tools like Java libraries, Java source compilers, Java debuggers, bundling and deployment tools.
- Just In Time compiler (JIT) is runs after the program has started executing, on the fly. It has access to runtime information and makes optimizations of the code for better performance.

[Differentiate JVM JRE JDK JIT](https://javapapers.com/core-java/differentiate-jvm-jre-jdk-jit/)  


    import java.io.Serializable;

    public class Car implements Serializable {
        //Private Properties
        private String color;
        private Boolean isCar;

        //Zero-argument Constructor
        public Car(){}

        //Getter and Setter
        public void setColor(String color) { this.color = color; }
        
        public String getColor() { return color; }
        
        public void setCar(Boolean car) { isCar = car; }

        //'is' for Boolean getter
        public Boolean isCar() { return isCar; }
    }

### Bean Properties

{{% alert note %}}
- Read and write property has getter and setter 
- A read-only property has a getter method but no setter
- A write-only property has a setter method only
- Boolean property using is instead of get
{{% /alert %}}

- Indexed Properties  
an array instead of a single value
- Bound Properties  
PropertyChangeListeners
- Constrained Properties  
VetoableChangeListeners

### Bean Methods

Any public method that is not part of a property definition is a bean method. 

### Bean Events

- A bean class can fire off any type of event
- Method names with a specific pattern
- Can be used in wiring components together

### BeanInfo

A BeanInfo is a class that changes how your bean appears in a builder tool.

### Bean Persistence

#### Serialization

A bean has the property of persistence when its properties, fields, and state information are saved to and retrieved from storage.  
All beans must persist. To persist, must implement either of below:  
- java.io.Serializable
- java.io.Externalizable

Any class is serializable as long as that class or a parent class implements the java.io.Serializable interface.  
Examples:  
- Component
- String
- Date
- Vector
- Hashtable

Not serializable:  
- Image
- Thread
- Socket
- InputStream

Controlling Serialization:  
- Automatic serialization
- Customized serialization
- Customized file format

#### Long Term Persistence

Long-term persistence is a model that enables beans to be saved in XML format.

### Links
[JavaBeans](https://en.wikipedia.org/wiki/JavaBeans)  
[Oracle's JavaBeans tutorials](http://download.oracle.com/javase/tutorial/javabeans/)  
[JavaBeans specification](http://www.oracle.com/technetwork/java/javase/documentation/spec-136004.html)  
[初识Spring —— Bean的装配（一）](https://juejin.im/post/6844903618567471112)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌
