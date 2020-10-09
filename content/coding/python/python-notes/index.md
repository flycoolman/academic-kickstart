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

{{% alert note %}}
TBD
{{% /alert %}}

Pass By Value and Pass By Reference and Pass Reference by Value
[Java Pass By Value and Pass By Reference](https://javapapers.com/core-java/java-pass-by-value-and-pass-by-reference/)  


<br>

#### Did you find this page helpful? Consider sharing it 🙌
