---
title: 'Java Lambda Expression'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A brief description of Java collections.
authors:
- admin
tags:
- Java
categories:
- Java
date: "2019-12-31T23:59:59Z"
lastmod: "2020-01-01T00:00:00Z"
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

# Java Collections

- Any group of individual objects which are represented as a single unit is known as the collection of the objects.  
- The **Collection in Java is a framework that provides an architecture to store and manipulate the group of objects.  
- The **Collection** interface (**java.util.Collection**) and Map interface (**java.util.Map**) are the two main “root” interfaces of Java collection classes.  
- Java Collections can achieve all the operations that you perform on a data such as **searching**, **sorting**, **insertion**, **manipulation**, and **deletion**.

## Java Collection Hierarchy

![java-collection-hierarchy](./java-collection-hierarchy.png)  
![java-collections-hierarchy-1](./java-collections-hierarchy-1.png)  
![java-collections-hierarchy-2](./java-collections-hierarchy-2.jpg) 

The utility package, (java.util) contains all the classes and interfaces that are required by the collection framework. The collection framework contains an interface named as an iterable interface which provides the iterator to iterate through all the collections. This interface is extended by the main collection interface which acts as a root for the collection framework. All the collections extend this collection interface thereby extending the properties of the iterator and the methods of this interface.  

### Iterable Interface

This is the root interface for the entire collection framework. The collection interface extends the iterable interface. Therefore, inherently, all the interfaces and classes implement this interface. The main functionality of this interface is to provide an iterator for the collections. Therefore, this interface contains only one abstract method which is the iterator. It returns the

    Iterator iterator();

### List Interface

- This interface is dedicated to the data of the list type in which we can store all the ordered collection of the objects.  
- This also allows duplicate data to be present in it.  
- It is implemented by various classes like **ArrayList**, **Vector**, **Stack**, etc.  

#### ArrayList

- ArrayList provides us with **dynamic arrays** in Java. Though, it may be slower than standard arrays but can be helpful in programs where lots of manipulation in the array is needed. The size of an ArrayList is increased automatically if the collection grows or shrinks if the objects are removed from the collection.  
- It is like an array, but there is no size limit.  
- Java ArrayList allows us to randomly access the list. ArrayList can not be used for primitive types, like int, char, etc. We will need a wrapper class for such cases.  
- The ArrayList class maintains the insertion order and is non-synchronized.  
- The elements stored in the ArrayList class can be randomly accessed.  

#### LinkedList

- LinkedList is a linear data structure where the elements are not stored in contiguous locations and every element is a separate object with a data part and address part.  
- The elements are linked using pointers and addresses. Each element is known as a node.
- It uses a doubly linked list internally to store the elements.  
- It can store the duplicate elements.  
- It maintains the insertion order and is not synchronized.  
- In LinkedList, the manipulation is fast because no shifting is required.  


#### Vector

- Vector uses a dynamic array to store the data elements.  
- It may be slower than standard arrays but can be helpful in programs where lots of manipulation in the array is needed. 
- The primary difference between a vector and an ArrayList is that a Vector is synchronized and an ArrayList is non-synchronized.  
- It is synchronized and contains many methods that are not the part of Collection framework.  

#### Stack

- It implements the last-in-first-out data structure. 
- The stack is the subclass of Vector. The stack contains all of the methods of Vector class and also provides its methods like boolean push(), boolean peek(), boolean push(object o), which defines its properties.  

{{% alert note %}}
Stack is a subclass of Vector and a legacy class. It is thread safe which might be an overhead in an environment where thread safety is not needed.  
An alternate to Stack is to use ArrayDequeue which is not thread safe and faster array implementation.
{{% /alert %}}


### Queue Interface

A queue interface maintains the FIFO(First In First Out) order similar to a real-world queue line. This interface is dedicated to storing all the elements where the order of the elements matter. There are various classes like **PriorityQueue**, **Deque**, **ArrayDeque**, etc.  

#### Priority Queue

- A PriorityQueue is used when the objects are supposed to be processed based on the priority.  
- It is known that a queue follows the First-In-First-Out algorithm, but sometimes the elements of the queue are needed to be processed according to the priority and this class is used in these cases.  
- The PriorityQueue is based on the priority heap.  
- The elements of the priority queue are ordered according to the natural ordering, or by a Comparator provided at queue construction time, depending on which constructor is used.
-  PriorityQueue doesn't allow null values to be stored in the queue.  


### Deque Interface

Deque, also known as a double-ended queue, is a data structure where we can add and remove the elements from both the ends of the queue.  
- This interface extends the queue interface.  
- The class which implements this interface is **ArrayDeque**.  


#### ArrayDeque

- ArrayDeque class which is implemented in the collection framework provides us with a way to apply resizable-array. This is a special kind of array that grows and allows users to add or remove an element from both sides of the queue.  
- Array deques have no capacity restrictions and they grow as necessary to support usage.  
- ArrayDeque is faster than ArrayList and Stack.   


### Set Interface

- A set is an unordered collection of objects in which duplicate values cannot be stored.  
- This collection is used when we wish to avoid the duplication of the objects and wish to store only the unique objects.  
- This set interface is implemented by various classes like **HashSet**, **TreeSet**, **LinkedHashSet**, etc.  

#### HashSet

- The HashSet class is an inherent implementation of the hash table data structure.  
- The objects that we insert into the HashSet do not guarantee to be inserted in the same order. The objects are inserted based on their hashcode.  
- This class also allows the insertion of NULL elements.  

#### LinkedHashSet

- LinkedHashSet uses a doubly linked list to store the data and retains the ordering of the elements.  
- Like HashSet, It also contains unique elements.  
- It maintains the insertion order. 
- It permits null elements.  

### SortedSet Interface

- This interface is very similar to the set interface. The only difference is that this interface has extra methods that maintain the ordering of the elements.  
- The sorted set interface extends the set interface and is used to handle the data which needs to be sorted.  
- The class which implements this interface is **TreeSet**.  

#### TreeSet

- The TreeSet class uses a Tree for storage.  
- The ordering of the elements is maintained by a set using their natural ordering whether or not an explicit comparator is provided.  
- It can also be ordered by a Comparator provided at set creation time, depending on which constructor is used.  
- the access and retrieval time of TreeSet is quite fast.  
- The elements in TreeSet stored in ascending order.  


### Map Interface

A map is a data structure which supports the key-value pair mapping for the data. This interface doesn’t support duplicate keys because the same key cannot have multiple mappings. A map is useful if there is a data and we wish to perform operations on the basis of the key. This map interface is implemented by various classes like **HashMap**, **TreeMap**, etc.  

#### HashMap

HashMap provides the basic implementation of the Map interface of Java. It stores the data in (Key, Value) pairs. To access a value in a HashMap, we must know its key. HashMap uses a technique called Hashing. Hashing is a technique of converting a large String to small String that represents the same String so that the indexing and search operations are faster. HashSet also uses HashMap internally.  


### A Little Deep In Queues

#### The queue (a FIFO list)  
Implementation of a queue
- A queue (of bounded size) can be efficiently implemented in an array.  
- A queue can be efficiently implemented using any linked list that supports deletion in the front and insertion at the end in constant time. The first (last) element of the queue is at the front (end) of the linked list.  

#### The stack (a LIFO list)

Implementation of a stack
- A stack (of bounded size) can be efficiently implemented using an array b and an int variable n: The n elements of the stack are in b[0..n-1], with b[0] being the bottom element and b[n-1] being the top element.  
- A stack can be efficiently implemented using a linked list. The first element is the top of the stack and the last element is the bottom. It’s easy to push (prepend) an element and pop (remove) the first element in constant time.  

#### The deque
The word deque, usually pronounced deck, is short for double-ended queue. A deque is a list that supports insertion and removal at both ends. Thus, a deque can be used as a queue or as a stack.  

#### Stacks, queues, and deques in the Java Collection framework
- Java has interface Deque<E>. It is implemented by classes ArrayDeque<E> (which implements a list in an expandable array) and LinkedList<E>, so these two classes can be used for a queue and for a stack.  
- Both ArrayDeque and LinkedList also implement interface Queue<E>, so you can use this interface to restrict operations to queue operations. For example, create a LinkedList and assign it to a Queue variable.  

        Queue<E> q= new LinkedList<>();  

Thereafter, use only q for the LinkedList and operations are restricted to queue operations.
- Java also has a class Stack<E>, which implements a stack in an expandable array. However, the Java API would rather you use an ArrayDeque. The problem is that there is no suitable way to restrict the operations of an Array-Deque to stack operations, so we prefer to use class Stack<E>.


## Links

[Collections in Java](https://www.geeksforgeeks.org/collections-in-java-2/)  



<br>

#### Did you find this page helpful? Consider sharing it 🙌
