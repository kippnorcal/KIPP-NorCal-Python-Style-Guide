# KIPP NorCal's Python Style Guide

**TABLE OF CONTENTS**
- [Introduction](#intro)
  - [Breaking Conventions](#breaking-conventions)
  - [Consistency](#consistency)
- [Naming Conventions](#naming-conventions)
  - [Names to Avoid](#names-avoid)
  - [Package and Modules](#packages-modules)
  - [Classes](#classes)
  - [Methods/Functions](#methods-functions)
    - [Leading and Trailing Underscores](#underscores)
    - [Function/Method Arguements](#func-method-args)
  - [Variables](#variables)
- [Imports](#variables)
  - [Import Practices](#import-practices)
  - [Import Location and Organization](#import-organization)
  - [Wildcard Imports Using *](#wildcard-imports)
- [Boolean and None Type Testing](#boolean-none-comparisons)
- [Type Hinting](#type-hinting-syntax)
  - [Type Hinting Syntax](#type-hinting-syntax)
- [Comments](#comments)
  - [Single Line Comments](#single-line-comments)
  - [Docstrings](#docstrings)
- [Exception Handling](#exception-handling)
- [Opening Files/Writing to Files](#file-io)
- [Quotes](#quotes)
- [Multi-line Signatures and Function Calls](#multi-line)
- [Line Length](#line-length)



## Introduction<a id="intro"></a>

There are already many style guides out there for Python, so instead of re-creating the wheel, this document borrows
_(i.e. copy and paste)_ heavily from them, with a few tweaks here and there. The styles captured here are to emphasize 
them because of their usefulness to creating consistent, maintainable, and readable code.

There are also a lot of styles not captured here that are still worth knowing about, and reading the following guides 
is highly encouraged. The guides listed below are in agreement on many coding conventions, but offer different 
perspectives on their reasoning for certain styles.

- [PEP 8](https://pep8.org/) - The official style guid of the Python community
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html) - If it’s good for Google, it should be good enough for us
- [The Hitchhiker's Guide to Python](https://docs.python-guide.org/writing/style/) 

### Breaking Conventions<a id="breaking-conventions"></a>
Don't. Some of the above guides (specifically Google's) give leeway to breaking conventions. The most valid reason for 
this has to deal with backwards compatibility. Backwards compatibility is not something we need to worry to deal with 
at our organization.

There is a lot of code that existed at KIPP NorCal before this style guide, and so there will be code that doesn't 
conform this guide. Whenever refactoring this code, we should also work to clean up areas that don't follow convention.

### Consistency<a id="consistency"></a>
There are some conventions that will lay out multiple options for a style convention (although, usually no more than 
two). Which ever option you choose to use when building new code, use it consistently. Do not switch between them. 
When working on existing code, stick to the convention that the original author chose to use.

## Naming Conventions<a id="naming-conventions"></a>
We will follow what [PEP 8](https://pep8.org/#naming-conventions) has laid out for naming conventions. Below is a copy 
and paste of some key points.

### Names to Avoid<a id="names-avoid"></a>
Never use the characters ‘l’ (lowercase letter el), ‘O’ (uppercase letter oh), or ‘I’ (uppercase letter eye) as 
single character variable names.

In some fonts, these characters are indistinguishable from the numerals one and zero. When tempted to use ‘l’, use ‘L’ 
instead.

### Package and Modules<a id="packages-modules"></a>
Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability. 
Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.

### Classes<a id="classes"></a>
Class names should normally use the CapWords _(aka CamelCase)_ convention.

There is a note in [PEP 8](https://pep8.org/#class-names) where _"The naming convention for functions may be used 
instead"_. Disregard this. We will always use CapWords.

### Methods/Functions<a id="methods-functions"></a>
Function names should be lowercase, with words separated by underscores.

There is a note in [PEP 8](https://pep8.org/#function-names) where underscores are used optionally to improve 
readability. Disregard this. Underscores always improve readability.

#### Leading and Trailing Underscores<a id="underscores"></a>
Use single leading underscore names to denote a method or a function as private. Additionally from [PEP 8](https://pep8.org/#descriptive-naming-styles) - _"weak “internal use” 
indicator. E.g. from M import * does not import objects whose name starts with an underscore"_.
```python
def _single_leading_underscore() -> None:
    """This will not get imported.""" 
    return None
```
Use single trailing underscore to avoid conflicts with Python keywords: 
```python
def single_trailing_underscore_(x: int) -> bool:
    class_ = 5  # Can use with variables, too! 
    if x == class_:
        return True
    else:
        return False
```


#### Fucntion/Method Arguements<a id="func-method-args"></a>
Always use self for the first argument to instance methods.

Always use cls for the first argument to class methods.

If a function argument’s name clashes with a reserved keyword, it is generally better to append a single trailing 
underscore rather than use an abbreviation or spelling corruption. Thus class_ is better than clss. (Perhaps better is 
to avoid such clashes by using a synonym.)

### Variables<a id="variables"></a>
Variables should follow the naming conventions of methods/functions.

## Imports <a id="variables"></a>
Follow the guide for imports in [PEP 8](https://pep8.org/#imports). In general, here are the key points:

#### Import Practices<a id="import-practices"></a>
Importing modules or pakages need their own line.

__BAD__
```python
import os, sys
```

__GOOD__
```python
import os
import sys
```
Importing multiple items that are contained within a module or package on one line is okay.
```python
# Both acceptable
from os import chdir, getcwd 

from os import chdir
from os import getcwd
```
#### Import Location and Organization<a id="import-organization"></a>
Imports should always be at the top of a file after any module docstrings and before any module globals or constants 
are defined.

Imports should be grouped into three groups with a blank line between them. Within the groups, it is recommended to 
alphabetize the imports by module or package. The three groupings are:

1. standard library imports
2. related third party imports
3. local application/library specific imports

Example:
```python
"""
Module docstring
"""

import datetime
import os

import pandas
import requests

from some_local_module import foo

SOME_CONSTANT = None
```

#### Wildcard Imports Using *<a id="wildcard-imports"></a>
Avoid using * when importing. This method of import loads everything from that module or package into your module's 
name space. In most cases, this is unecessary. If you only need one object from a package or module, then explicitly 
import that object instead of everything with a wildcard. If you do need everything from a package or module, 
then import the package or module to avoid namespace issues.

__EXAMPLE__
```python
# instead of this
from os import *

# do this if you need all of the os library
import os

# or this if you need one function
from os import getcwd
```
Exception to this rule is when using the Django (Galaxy). It is common to import models into `views.py` or importing views into 
`urls.py` using *.


## Boolean and None Type Testing<a id="boolean-none-comparisons"></a>

## Type Hinting<a id="type-hinting-syntax"></a>
Type hinting is a little controversial in the Python community. Many people feel that it ges against dynamic typing, 
which is one of the features of Python that makes it unique. 

For KIPP NorCal, many of our repos are designed for our 
own business purposes with a very specific purpose in mind, and type hinting can be a useful way to document code. 
Type hits can also speed up code development with auto-completion in IDEs. Type hints can also help catch bugs when 
used with a linter (such as [Pylint](https://pypi.org/project/pylint/) or [Flake8](https://flake8.pycqa.org/en/latest/)). 

### Type Hinting Syntax<a id="type-hinting-syntax"></a>
The syntax for type hints was defined in [PEP 3107](https://peps.python.org/pep-3107/#syntax). Below are some examples:

```python
def simple_example(a: str) -> None:
    """Takes one string param (a) and returns None"""
    print(a)
    

def foo(a: str, b: bool = False) -> bool:
    """
    Takes a string parameter with a boolean parameter defaulting to False. 
    Returns a boolean.
    """
    if b:
        return True
    else:
        return False
    
    
from typing import Union

def bar(a: Union[int, float]) -> Union[None, int]:
    """
    One parameter (a) which can be an integer or a float.
    Returns either an integer or None.
    """
    if isinstance(a, int):
        return a
    else:
        return None
```


## Comments<a id="comments"></a>
The general rule of thumb for good commenting is that your comments should be adding additional context that might not 
be apparent in the code. Comments not restate exactly what your code is doing.

__BAD__
```python
# print results
print(results)
```

### Single Line Comments<a id="single-line-comments"></a>
TODO: Add content 

### Docstrings<a id="docstrings"></a>
For docstrings, follow [PEP 257](https://peps.python.org/pep-0257/). 

Here is an example for a single line docstring:
```python
def foo():
    """A single line docstring."""
```

Here is an example of a multi-line docstrings These are equivalent:
```python
def foo():
    """A multi-line
    docstring.
    """

def bar():
    """
    A multi-line
    docstring.
    """
```

## Exception Handling<a id="exception-handling"></a>
Whenever handling exceptions with a try/except block, do not use a bare exception as this can hide bugs in your code. 
Always capture specific exceptions.

__BAD__
```python
def raises_an_exception(some_list):
    try:
        return some_list[1000]
    except:     # or except Exception:
        return None
```

__GOOD__
```python
def raises_an_exception(some_list):
    try:
        return some_list[1000]
    except IndexError:
        return None
```
The one exception to this rule is with the `if __name__ == '__main__'`. The bare try/except blocks here capture the 
error and log it before terminating teh code.

## Opening Files/Writing to Files<a id="file-io"></a>

Whenever opening files, always use a context manager.  With a context manager, you never have to worry about closing 
the file when your code is done, which reduces the risk of corrupting your file.

__BAD__
```python
f = open('file.txt')
a = f.read()
print(a)
f.close()
```

__GOOD__
```python
with open('file.txt') as f:
    for line in f:
        print(line)
```

## Quotes<a id="quotes"></a>
TODO: Add content here

## Multi-line Signatures and Function Calls<a id="multi-line"></a>
TODO: Add content here

## Line Length<a id="line-length"></a>
TODO: Add content here
