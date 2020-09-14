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
date: "2019-12-31T23:59:59Z"
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

## Java Notes
A free style notes of Java.

### Differentiate JVM JRE JDK JIT

- Java Virtual Machine (JVM) is an abstract computing machine.
- Java Runtime Environment (JRE) is an implementation of the JVM.
- Java Development Kit (JDK) contains JRE along with various development tools like Java libraries, Java source compilers, Java debuggers, bundling and deployment tools.
- Just In Time compiler (JIT) is runs after the program has started executing, on the fly. It has access to runtime information and makes optimizations of the code for better performance.

[Differentiate JVM JRE JDK JIT](https://javapapers.com/core-java/differentiate-jvm-jre-jdk-jit/)  

### Pass By Value and Pass By Reference and Pass Reference by Value

{{% alert note %}}
Java uses pass by value. There is no pass by reference in Java.
{{% /alert %}}

Pass By Value and Pass By Reference and Pass Reference by Value
[Java Pass By Value and Pass By Reference](https://javapapers.com/core-java/java-pass-by-value-and-pass-by-reference/)  


- Java always passes parameter variables by value.
- Object variables in Java always point to the real object in the memory heap.
- A mutable object’s value can be changed when it is passed to a method.
- An immutable object’s value cannot be changed, even if it is passed a new value.
- “Passing by value” refers to passing a copy of the value.
- “Passing by reference” refers to passing the real reference of the variable in memory.

[Does Java pass by reference or pass by value?](https://www.infoworld.com/article/3512039/does-java-pass-by-reference-or-pass-by-value.html#:~:text=Java%20always%20passes%20parameter%20variables,is%20passed%20to%20a%20method.)  


### Java (JVM) Memory Types

**Shared/Common Area** 
1. Heap Memory  
Class instances and arrays are stored in heap memory. Heap memory is also called as shared memory. As this is the place where multiple threads will share the same data.  
Heap data area is created at VM startup. Claiming the memory back is done automatically by the garbage collector (GC).

2. Non-heap Memory  
    * Method area 
    Method area is created at JVM startup and shared among all the threads. 
        1. per-class structures (runtime constants and static fields)  
        2. code for methods  
        3. constructors
        4. **Run-time Constant Pool**  

**Per-Thread Area**
1. Program Counter (PC) Register
PC keeps a pointer to the current statement that is being executed in its thread. If the current executing method is ‘native’, then the value of program counter register will be undefined.
2. JVM Stacks or Frames
Java JVM frames are created when a method is invoked, it performs the dynamic linking. JVM stacks are created and managed for each thread.
3. Native Method Stacks
It is used for native methods, and created per thread. 

**Memory Generations**
HotSpot VM’s garbage collector uses generational garbage collection. It separates the JVM’s memory into and they are called young generation and old generation.
1. Young Generation
- Eden space
- Survivor space
2. Old Generation
- Tenured Generation
GC moves live objects from survivor space to tenured generation. 
- PermGen (Permanent Generation)
The permanent generation contains meta data of the virtual machine, class and method objects.  

(Java JVM Run-time Data Areas)[https://javapapers.com/core-java/java-jvm-run-time-data-areas/#Java_Virtual_Machine_Stacks]  
(Java (JVM) Memory Types)[https://javapapers.com/core-java/java-jvm-memory-types/]  

**Key Takeaways**  
- Local Variables are stored in Frames during runtime.
- Static Variables are stored in Method Area.
- Arrays are stored in heap memory.

### Java Static

#### Java Static Variables
- Java instance variables are given separate memory for storage. If there is a need for a variable to be common to all the objects of a single java class, then the static modifier should be used in the variable declaration.
- Any java object that belongs to that class can modify its static variables.
- Also, an instance is not a must to modify the static variable and it can be accessed using the java class directly.
- Static variables can be accessed by java instance methods also.
- When the value of a constant is known at compile time it is declared ‘final’ using the ‘static’ keyword.

####Java Static Methods
- Similar to static variables, java static methods are also common to classes and not tied to a java instance.
- Good practice in java is that, static methods should be invoked with using the class name though it can be invoked using an object. ClassName.methodName(arguments) or objectName.methodName(arguments)
- General use for java static methods is to access static fields.
- Static methods can be accessed by java instance methods.
- Java static methods cannot access instance variables or instance methods directly.
- Java static methods cannot use the ‘this’ keyword.
#### Java Static Classes
- For java classes, only an inner class can be declared using the static modifier.  
- For java a static inner class it does not mean that, all their members are static. These are called nested static classes in java.  


<br>

#### Did you find this page helpful? Consider sharing it 🙌
