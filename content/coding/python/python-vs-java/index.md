---
title: 'Python vs. Java'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A VS. notes between Python and Java.
authors:
- admin
tags:
- Python
- Java
categories:
- Coding
- Python
- Java
date: "2019-12-31T23:59:59Z"
lastmod: "2020-01-01T00:00:00Z"
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

## Python vs. Java
A VS. notes between Python and Java.

### Type casting

- Python: **int(1.9)**, **float(1)**, **str(1.9)**    
- Java:  
    * Narrowing casting: (manually) - converting a larger type to a smaller size type
      **(int)1.9**, **(float)9.78**    
      double -> float -> long -> int -> char -> short -> byte  
    * Widening casting: automatically - converting a smaller type to a larger type size  
      byte -> short -> char -> int -> long -> float -> double  

### True or False

- Python: True/False  
- Java: true/false   

### Map sorting

- Python:  
Easy to use sorted() and lambda to sort dict/map by keys or values  
- Java:  
Use Collections.sort() and Comparator to sort map by keys or values  
Java 8 provides new ways of defining Comparators by using lambda expressions and the comparing() static factory method.  


### Python max() vs Java Math.max() vs Java Collections.max()

- Python max()  
Can campare multiple items, or iterable, even strings  

        max(n1, n2, n3, ...)
or

        max(iterable)
or **Compare strings**  

        max("Mike", "John", "Vicky")

- Java max()  
Only compare two mathematic items, i.e. int, float, double ...  

- Java Collections.max()  

max(Collection<? extends T> coll)  
Returns the maximum element of the given collection, according to the natural ordering of its elements.  
static <T> T	max(Collection<? extends T> coll, Comparator<? super T> comp)  
Returns the maximum element of the given collection, according to the order induced by the specified comparator.  
**Compare Integers**  

        Integer[] num = { 2, 4, 7, 5, 9 };
        // using Collections.min() to
        // find minimum element
        // using only 1 line.
        int min = Collections.min(Arrays.asList(num));
        // using Collections.max()
        // to find maximum element
        // using only 1 line.
        int max = Collections.max(Arrays.asList(num));

or **Compare Strings**  

        // List<String> list = new ArrayList<String>(Arrays.asList("Mike Smith", "John", "Vicky"));
        List<String> list = Arrays.asList("Mike", "John", "Vicky");
        String max = Collections.max(list);
or **Use comparator**  

        List<String> list = Arrays.asList("Mike Smith", "John", "Vicky");
        String max = Collections.max(list, Comparator.comparing(s -> s.length()));

{{% alert note %}}
Python - Java  
The aboves are same to min().  
{{% /alert %}}

### Infinity

- Python  
    * math.inf
      The **math.inf** constant returns a floating-point positive infinity.
      For negative infinity, use **-math.inf**.
      The inf constant is equivalent to float('inf').
      
        import math
        print(math.inf)
    * sys.maxsize  
    An integer giving the maximum value a variable of type Py_ssize_t can take. It’s usually 2**31 - 1 on a 32-bit platform and 2**63 - 1 on a 64-bit platform.  
    like sys.maxint in Python2.  

- Java  
    * Integer.MAX_VALUE, Integer.MIN_VALUE, Integer.MAX_VALUE+1, Integer.MIN_VALUE-1  
    * Long.MAX_VALUE, Long.MIN_VALUE  
    * Float.MAX_VALUE, Float.MIN_VALUE, Float.POSITIVE_INFINITY, Float.NEGATIVE_INFINITY  
    * Double.MAX_VALUE, Double.MIN_VALUE, Double.POSITIVE_INFINITY, Double.NEGATIVE_INFINITY, Double.NaN


{{% alert note %}}
There is no way to represent infinity as an integer in Python. This matches the behaviour of many other languages. However, due to Python's dynamic typing system, you can use float('inf') in place of an integer, and it will behave as you would expect.
{{% /alert %}}

{{% alert note %}}
To Be Continued
{{% /alert %}}

### Map/Dict Composite key

- Python  

        map = {(1, 0) : 2, (1, 1) : 3, (2, 0) : 4, (2, 1) : 5}
        map[(1,0)]
        map.get((1,0))

- Java  
Not so easy to do

### String equals vs ==

Check if the characters in two strings are identical  

- Python ==  

        s1 = "Hello"
        s2 = "Hello"
        s3 = "Hello3"
        s4 = "Hell" + "o"
        s5 = "Hell"
        s5 += "o"
        id(s1)                 #  140701928086752
        id(s2)                 #  140701928086752
        id(s3)                 #  140701928160032
        id(s4)                 #  140701928086752
        id(s5)                 #  140701884560640
        s1 == s2               #  True
        s1 == s3               #  False
        s1 == s4               #  True
        s1 == s5               #  True

- Java equals() vs ==  

**==** to compare memory address or say identity in system
**equals()** to compare the content of strings  

        String s1 = "Hello";
        String s2 = "Hello";
        String s3 = "Hello3";
        String s4 = "Hell" + "o";
        String s5 = "Hell";
        s5 = s5 + "o";
        System.out.println(Integer.toHexString(s1.hashCode()));                //  42628b2
        System.out.println(Integer.toHexString(s2.hashCode()));                //  42628b2
        System.out.println(Integer.toHexString(s3.hashCode()));                //  809eedc1
        System.out.println(Integer.toHexString(s4.hashCode()));                //  42628b2
        System.out.println(Integer.toHexString(s5.hashCode()));                //  42628b2
        System.out.println(Integer.toHexString(System.identityHashCode(s1)));  //  2038ae61
        System.out.println(Integer.toHexString(System.identityHashCode(s2)));  //  2038ae61
        System.out.println(Integer.toHexString(System.identityHashCode(s3)));  //  3c0f93f1
        System.out.println(Integer.toHexString(System.identityHashCode(s4)));  //  2038ae61
        System.out.println(Integer.toHexString(System.identityHashCode(s5)));  //  31dc339b
        System.out.println(s1 == s2);                                          //  true
        System.out.println(s1 == s3);                                          //  false
        System.out.println(s1 == s4);                                          //  true
        System.out.println(s1 == s5);                                          //  false, not same identity
        System.out.println(s1.equals(s2));                                     //  true
        System.out.println(s1.equals(s3));                                     //  false
        System.out.println(s1.equals(s4));                                     //  true
        System.out.println(s1.equals(s5));                                     //  true, contents are equal




<br>

#### Did you find this page helpful? Consider sharing it 🙌
