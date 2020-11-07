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

### 

{{% alert note %}}
To Be Continued
{{% /alert %}}




<br>

#### Did you find this page helpful? Consider sharing it 🙌
