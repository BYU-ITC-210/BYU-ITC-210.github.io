---
title: Python Walkthrough
---

## Prepare Environment

**Install Python**

* Windows guidelines: https://docs.microsoft.com/en-us/windows/python/beginners
    * Installing from the Windows store instead of https://python.org will automatically add Python to your path and set up automatic updates.
* Mac OS X and Linux: Install from https://python.org

Get the `Requests` package

* From the command-line (cmd, powershell, bash): `pip install Requests`

Launch Python – results in a Python interpreter command-line

## Check Your Version 

```python
>>> import sys
>>> sys.version
```

## Basics – Interactive Python

```python
>>> 2+8
10
```

Multi-line input (continues until the expression is complete)

```
>>> (2+8
... )*3
30
```

## Variables 

Variables are automatically created when you assign a value.

```
>>> spam = 2+8
>>> spam
10
```

In interactive mode, you simply type a variable or expression and it will interpret it. In a program you 
use the print function.

```
>>> print(spam/2)
5.0
```

## Functions 

Use the “def” keyword to indicate the definition of a function

```
>>> def difference(x,y):
...   return abs(x-y)
...
```

Indentation is meaningful in Python, not just helpful to the programmer.

Calling the function:

```
>>> difference(3,9)
6
```

## Scope and Dynamic Binding 

Three scopes: 

* Local (within a function)
* Global (within a file or interactive session)
* Built-in

Nearest scope (local, global, or built-in) takes precedence.

```
>>> abs = 5
```

Setting a global variable to an integer overrides the built-in function abs.

```
>>> abs(-2)
Traceback (most recent call last):
 File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```

You cannot call an integer value like a function.

```
>>> difference(2,7)
Traceback (most recent call last):
 File "<stdin>", line 1, in <module>
 File "<stdin>", line 2, in difference
TypeError: 'int' object is not callable
```

Dynamic binding means it affects the already built difference function.

```
>>> del abs
```

Deleting the global variable restores built-in scope to be the resolution of abs.

```
>>> abs(-2)
2
>>> difference(2,7)
5
```

Like JavaScript, functions are assigned to variables and referenced accordingly.

## Modules 
A module is a python file that containing code to be imported.

emphasis.py Module

```
def strong(x):
  return x + "!"

def question(x):
  return x + "?"
```

Using the module

```
>>> import sys
>>> sys.path.append('C:\\Users\\YourName\\Source\\Python')
>>> import emphasis
>>> emphasis.strong('Hello')
'Hello!'
>>> emphasis.question('Hello')
'Hello?'
```

## Lists and Loops 

```
>>> items = ['rabbit', 'grail', 3, 1.5]
>>> for x in items:
...   print(x)
...
# Output
>>> for x in items:
...   print(type(x))
...
# Output
```

## Conditionals

```
>>> def isbig(x):
...   if x > 100:
...     print(str(x) + ' is big.')
...   else:
...     print(str(x) + ' is small.')
...
>>> isbig(20)
# Output
>>> isbig(200)
# Output
```

## Accessing REST APIs 

We’ll be using the Quotes REST API at [https://quotes.rest](https://quotes.rest)

```
>>> import requests
>>> res = requests.get('https://quotes.rest/qod')
>>> res.status_code
# Output
>>> print(res.text)
# Output
>>> res.json()['contents']['quotes'][0]['quote']
# Output
>>> res.json()['contents']['quotes'][0]['author']
# Output
```

(If `import requests` generates an error, you have not installed the `Requests` module. From the command line (not Python) type `pip install Requests`.

## Docstrings

A string constant as the first statement in a function or module provides a description of what that 
function or module does.

```
def strong(x):
 """Add emphasis to a statement."""
 return x + "!"
```

Triple-quotes allow multi-line string constants. It’s traditional to use triple quotes in a Docstring but 
single or double quotes are also acceptable.