---
title: 'Python Notes'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A free style notes of Python.
authors:
- admin
tags:
- Python
categories:
- Python
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

## Python Notes
A free style notes of Python.

### Class or Static Variables in Python

In C++ and Java, we can use static keyword to make a variable as class variable. The variables which don’t have preceding static keyword are instance variables.  

The Python approach is simple, it **doesn’t** require a static keyword. All variables which are assigned a value in class declaration are class variables. And variables which are assigned values inside methods are instance variables.  

### True or False

**Most Values are True:** 
- Almost any value is evaluated to True if it has some sort of content.  
- Any string is True, except empty strings.  
- Any number is True, except 0.  
- Any list, tuple, set, and dictionary are True, except empty ones.  

**Some Values are False:**  
In fact, there are not many values that evaluates to False,  
- except empty values, such as (), [], {}, "",  
- the number 0, and the value None.  
- And of course the value False evaluates to False.  

### Casting

- **int()** - constructs an integer number from an integer literal, a float literal (by rounding down to the previous whole number), or a string literal (providing the string represents a whole number)  
- **float()** - constructs a float number from an integer literal, a float literal or a string literal (providing the string represents a float or an integer)  
- **tr()** - constructs a string from a wide variety of data types, including strings, integer literals and float literals  

### Dictionary: [] vs. update
[How can I add new keys to a dictionary?](https://stackoverflow.com/questions/1024847/how-can-i-add-new-keys-to-a-dictionary)  
- **[]**  
you create a new key\value pair on a dictionary by assigning a value to that key. If the key doesn't exist, it's added and points to that value. If it exists, the current value it points to is overwritten.  
the d[key]=val syntax as it is shorter and can handle any object as key (as long it is hashable), and only sets one value

- **update**  
Whereas the .update(key1=val1, key2=val2) is nicer if you want to set multiple values at the same time, as long as the keys are strings (since kwargs are converted to strings).  
{{% alert note %}}
dict.update can also take another dictionary, but I personally prefer not to explicitly create a new dictionary in order to update another one. 
{{% /alert %}}

### collections
[collections — Container datatypes](https://docs.python.org/3/library/collections.html)  
This module implements specialized container datatypes providing alternatives to Python’s general purpose built-in containers, dict, list, set, and tuple.  

![python-collections](./python-collections.png)

{{% alert note %}}
Deprecated since version 3.3, will be removed in version 3.10: Moved Collections Abstract Base Classes to the collections.abc module. For backwards compatibility, they continue to be visible in this module through Python 3.9.
{{% /alert %}}


### collections.abc
collections.abc — Abstract Base Classes for Containers  

{{% alert note %}}
TBD
{{% /alert %}}


[Just a Placeholder](https://flycoolman.com)  


<br>

#### Did you find this page helpful? Consider sharing it 🙌