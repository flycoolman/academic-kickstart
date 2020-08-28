---
title: 'Java Beans'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A summary of Java Beans.
authors:
- admin
tags:
- Java
categories:
- Java
date: "2020-07-17T00:00:00Z"
lastmod: "2020-08-22T00:00:00Z"
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

## Java Beans

**Software Components**  

JavaBeans are classes that encapsulate many objects into a single object. They are serializable, have a zero-argument constructor, and allow access to properties using getter and setter methods. 

{{% alert note %}}
- a Java classs 
- serializable
- zero-argument constructor
- getter and setter
{{% /alert %}}

A bean is a Java class with method names that follow the JavaBeans guidelines. A bean builder tool uses introspection to examine the bean class. Based on this inspection, the bean builder tool can figure out the bean's properties, methods, and events.  
Almost any code can be packaged as a bean.   
The power of JavaBeans is that you can use software components without having to write them or understand their implementation.  

### Java Beans Example

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
