---
title: 'list, tuple, dict vs List, Tuple, Dict'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A little bit about Type Hints.
authors:
- admin
tags:
- Python
- Type hints
categories:
- Python
date: "2020-09-10T00:00:00Z"
lastmod: "2020-10-04T00:00:00Z"
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

### Type Hints

#### What is the challenge

Due to the dynamic nature of Python, inferring or checking the type of an object being used is especially hard. This fact makes it hard for developers to understand what exactly is going on in code they haven't written and, most importantly, for type checking tools found in many IDEs [PyCharm, PyDev come to mind] that are limited due to the fact that they don't have any indicator of what type the objects are. As a result they resort to trying to infer the type with (as mentioned in the presentation) around 50% success rate.

#### Type hinting

Type hinting is a formal solution to statically indicate the type of a value within your Python code. It was specified in [PEP 484](https://www.python.org/dev/peps/pep-0484/) 
and introduced in Python 3.5.  

Here’s an example of adding type information to a function. You annotate the arguments and the return value:

    def greet(name: str) -> str:
        return "Hello, " + name
The **name: str** syntax indicates the name argument should be of type str. The **->** syntax indicates the greet() function will return a string.  

<br>

**The below two statements can be found in the [Type Hinting](https://www.youtube.com/watch?v=2wDvzy6Hgxg) presentation:**

#### Why Type Hints

1. **Helps Type Checkers**: By hinting at what type you want the object to be the type checker can easily detect if, for instance, you're passing an object with a type that isn't expected.
2. **Helps with documentation**: A third person viewing your code will know what is expected where, ergo, how to use it without getting them TypeErrors.
3. **Helps IDEs develop more accurate and robust tools**: Development Environments will be better suited at suggesting appropriate methods when know what type your object is. You have probably experienced this with some IDE at some point, hitting the . and having methods/attributes pop up which aren't defined for an object.

#### Why use Static Type Checkers?

- **Find bugs sooner**: This is self evident, I believe.  
- **The larger your project the more you need it**: Again, makes sense. Static languages offer a robustness and control that dynamic languages lack. The bigger and more complex
  your application becomes the more control and predictability (from a behavioral aspect) you require. 
- **Large teams are already running static analysis**: I'm guessing this verifies the first two points.  

### Type Hinting with Mypy

[mypy](http://mypy-lang.org/index.html) is an optional static type checker for Python that aims to combine the benefits of dynamic (or "duck") typing and static typing. Mypy combines the expressive power and convenience of Python with a powerful type system and compile-time type checking. Mypy type checks standard Python programs; run them using any Python VM with basically no runtime overhead.

PEP 484 doesn't enforce anything; it is simply setting a direction for function annotations and proposing guidelines for how type checking can/should be performed. You can annotate your functions and hint as many things as you want; your scripts will still run regardless of the presence of annotations because Python itself doesn't use them.

As noted in the PEP, hinting types should generally take three forms:

- **Function annotations**. [PEP 3107](https://www.python.org/dev/peps/pep-3107/)  
- **Stub files** for built-in/user modules.  
- **Special # type**: type comments that complement the first two forms. (See: [What are variable annotations in Python 3.6?](https://stackoverflow.com/questions/39971929/what-are-variable-annotations-in-python-3-6/39973133#39973133) for a Python 3.6 update for **# type: type** comments)
Additionally, you'll want to use type hints in conjunction with the new typing module introduced in Py3.5.  

{{% alert note %}}
Additionally, you'll want to use type hints in conjunction with the new **typing module** introduced in Py3.5.
{{% /alert %}}
  

### The Typing Module

The Typing Module supports type hints as specified by [PEP 484](https://www.python.org/dev/peps/pep-0484/). 

The typing module also supports:

1. [Type aliasing](https://docs.python.org/3/library/typing.html#type-aliases)  
2. Type hinting for callback functions: [Callable](https://docs.python.org/3/library/typing.html#callable)  
3. [Generics](https://docs.python.org/3/library/typing.html#generics) - Abstract base classes have been extended to support subscription to denote expected types for container elements  
4. [User-defined generic types](https://docs.python.org/3/library/typing.html#user-defined-generic-types) - A user-defined class can be defined as a generic class  
5. [Any type](https://docs.python.org/3/library/typing.html#typing.Any) - Every type is a subtype of Any  

### Type Hinting Generics

    from typing import List
    class Solution:
        def twoSum(self, numbers: List[int], target: int) -> List[int]:
        

### List, Tuple/etc vs list/tuple/etc

typing.Tuple and typing.List are [Generic types](https://docs.python.org/3/library/typing.html#generics); this means you can specify what type their contents must be:

    def f(points: Tuple[float, float]):
        return map(do_stuff, points)

This specifies that the tuple passed in must contain two float values.  
**You can't do this with the built-in tuple type before Python 3.9.**  
**Python 3.9 has the change that built-in types support hints**

### Summary

{{% alert note %}}
You should always pick the typing generics even when you are not currently restricting the contents. It is easier to add that constraint later with a generic type as the resulting change will be smaller.
{{% /alert %}}

{{% alert note %}}
From Python 3.9 (PEP 585) onwards tuple, list and various other classes are now generic types. Using these rather than their typing counterpart is now preferred.
{{% /alert %}}

{{% alert note %}}
You should always pick then **non-typing generics** whenever possible as the old **typing.Tuple**, **typing.List** and **other generics** are **deprecated** and will be removed in a later version of Python.
{{% /alert %}}

### Links
[What are type hints in Python 3.5?](https://stackoverflow.com/questions/32557920/what-are-type-hints-in-python-3-5)
[PEP 585 -- Type Hinting Generics In Standard Collections](https://www.python.org/dev/peps/pep-0585/)
[typing — Support for type hints](https://docs.python.org/3.9/library/typing.html)
[Using List/Tuple/etc. from typing vs directly referring type as list/tuple/etc](https://stackoverflow.com/questions/39458193/using-list-tuple-etc-from-typing-vs-directly-referring-type-as-list-tuple-etc)
[Static typing in python3: list vs List](https://stackoverflow.com/questions/52629265/static-typing-in-python3-list-vs-list)  
[PEP 483 -- The Theory of Type Hints](https://www.python.org/dev/peps/pep-0483/)  
[PEP 484 -- Type Hints](https://www.python.org/dev/peps/pep-0484/)  
[Type Hints - Guido van Rossum - PyCon 2015](https://www.youtube.com/watch?v=2wDvzy6Hgxg)  
[Type Hinting](https://realpython.com/lessons/type-hinting/)  
[Type Checking With Mypy](https://realpython.com/lessons/type-checking-mypy/)  
[mypy](http://mypy-lang.org/index.html)  
<br>

#### Did you find this page helpful? Consider sharing it 🙌
