# KIPP NorCal's Python Style Guide

**TABLE OF CONTENTS**
- [Introduction](#intro)
  - [Breaking Conventions](#breaking-conventions)
  - [Consistency](#consistency)
- [Styles](#styles)
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
- [Coding Practices](#practices)



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

## Styles<a id="styles"></a>

#### Breaking Conventions<a id="breaking-conventions"></a>
Don't. Some of the above guides (specifically Google's) give leeway to breaking conventions. The most valid reason for 
this has to deal with backwards compatibility. Backwards compatibility is not something we need to worry to deal with 
at our organization.

There is a lot of code that existed at KIPP NorCal before this style guide, and so there will be code that doesn't 
conform this guide. Whenever refactoring this code, we should also work to clean up areas that don't follow convention.

#### Consistency<a id="consistency"></a>
There are some conventions that will lay out multiple options for a style convention (although, usually no more than 
two). Which ever option you choose to use when building new code, use it consistently. Do not switch between them. 
When working on existing code, stick to the convention that the original author chose to use.

### Naming Conventions<a id="naming-conventions"></a>
We will follow what [PEP 8](https://pep8.org/#naming-conventions) has laid out for naming conventions. Below is a copy 
and paste of some key points.

#### Names to Avoid<a id="names-avoid"></a>
Never use the characters ‘l’ (lowercase letter el), ‘O’ (uppercase letter oh), or ‘I’ (uppercase letter eye) as 
single character variable names.

In some fonts, these characters are indistinguishable from the numerals one and zero. When tempted to use ‘l’, use ‘L’ 
instead.

#### Package and Modules<a id="packages-modules"></a>
Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability. 
Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.

#### Classes<a id="classes"></a>
Class names should normally use the CapWords _(aka CamelCase)_ convention.

There is a note in [PEP 8](https://pep8.org/#class-names) where _"The naming convention for functions may be used 
instead"_. Disregard this. We will always use CapWords.

#### Methods/Functions<a id="methods-functions"></a>
Function names should be lowercase, with words separated by underscores.

There is a note in [PEP 8](https://pep8.org/#function-names) where underscores are used optionally to improve 
readability. Disregard this. Underscores always improve readability.

##### Leading and Trailing Underscores<a id="underscores"></a>
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


##### Fucntion/Method Arguements<a id="func-method-args"></a>
Always use self for the first argument to instance methods.

Always use cls for the first argument to class methods.

If a function argument’s name clashes with a reserved keyword, it is generally better to append a single trailing 
underscore rather than use an abbreviation or spelling corruption. Thus class_ is better than clss. (Perhaps better is 
to avoid such clashes by using a synonym.)

#### Variables<a id="variables"></a>
Variables should follow the naming conventions of methods/functions.

### Imports <a id="variables"></a>
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
##### Import Location and Organization<a id="import-organization"></a>
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

##### Wildcard Imports Using *<a id="wildcard-imports"></a>
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


### Boolean and None Type Testing<a id="boolean-none-comparisons"></a>
TODO: Add content here

### Type Hinting<a id="type-hinting-syntax"></a>
Type hinting is a little controversial in the Python community. Many people feel that it ges against dynamic typing, 
which is one of the features of Python that makes it unique. 

For KIPP NorCal, many of our repos are designed for our 
own business purposes with a very specific purpose in mind, and type hinting can be a useful way to document code. 
Type hits can also speed up code development with auto-completion in IDEs. Type hints can also help catch bugs when 
used with a linter (such as [Pylint](https://pypi.org/project/pylint/) or [Flake8](https://flake8.pycqa.org/en/latest/)). 

#### Type Hinting Syntax<a id="type-hinting-syntax"></a>
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

### Comments<a id="comments"></a>
The general rule of thumb for good commenting is that your comments should be adding additional context that might not 
be apparent in the code. Comments not restate exactly what your code is doing.

__BAD__
```python
# print results
print(results)
```

#### Single Line Comments<a id="single-line-comments"></a>
TODO: Add content 

#### Docstrings<a id="docstrings"></a>
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

### Exception Handling<a id="exception-handling"></a>
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
The one exception to this rule is using a bare try/except block within the `if __name__ == '__main__'`. The bare try/except blocks here capture the 
error, log it, and send a Slack notification before terminating the code.

### Opening Files/Writing to Files<a id="file-io"></a>

Whenever opening files, always use the `with` command. This creates a context manager, which reads cleaner and reduces the risk of corrupting your file.

__BAD__
```python
f = open('file.txt', "w")
f.write("Hello World!")
f.close()
```

__GOOD__
```python
with open('file.txt') as f:
    f.write("Hello World!")
```

### Quotes<a id="quotes"></a>
Python doesn't have a stance on whether single or double quotes are better for strings. Since the vast majority of 
existing code uses double quotes, lets stick to using double quotes. 

### Multi-line Signatures and Function Calls<a id="multi-line"></a>
When the signature of a function or a call to a function exceeds the set line length, then separate the signature or call over multiple lines with each parameter getting it's own line.

__EXAMPLE__
```python
# Pretend these are really long

# Long function signature
def some_long_func(
        a: str, 
        b: str, 
        c: str,
        d: str
) -> None:
    # Do some stuff
    return None

# Long function call
some_long_func(
  "my", 
  "dog", 
  "eats", 
  "rocks"
)
```
You don't have to only do this when exceeding the line limit. If at any point you feel that breaking these up over 
multiple lines makes your code more readable, then have at it! 

### Line Length<a id="line-length"></a>
TODO: Add content here

# Coding Practices<a id="practices"></a>

## Repo Must-Haves

### Logging

### Readme

### Slack Notifications

## Development Workflow
Development should always be done on your local machine to avoid breaking production or losing data. The pipelines on our servers are for production and should always be on the main branch whenever possible.

A high level development workflow example:
1. Write a test spec for the product/feature you're building
2. Prep for development
   1. If new work, create a new repo and create a development branch
      1. Add branch protections (?)
   2. If refactoring/creating a feature, checkout whichever branch you are planning to develop off of (this should almost always be main), and run `git pull` to get the most up-to-date code. Then create your development branch
      1. Helpful hint is to use a Jira issue ID or a semvar version number in the name. This can help point to documentation for your work incase you forget what the branch was for or incase someone else needs to look at the code.
3. Write your code. Commit and push often. You'll be happy you did if anything happens to your computer.
4. Test your code. This can look different from project to project. Whatever you choose to do, make sure you are covering your edge cases and the code is working as expected.
5. Once done with dev and testing, open a PR in GitHub
6. Once the PR is approved and merged to main: 
   1. If new repo: 
      - ssh into server
      - run `git clone <git repo address>` in `jobs` directory
      - build docker image
      - schedule job to run in crontab 
   2. If existing repo:
      - ssh into server and navigate to the repo's directory (should be in `/home/data_admin/jobs`)
      - Make sure repo is on main branch and run `git pull`
      - Rebuild docker image
      - Check that the new image name matches the image name in the command in crontab, so it will still run as expected. If not, then rename the image or update the command

## Environments
We use Pipenv to manage our environments in dev and production. We'll give a brief overview below, but Pipenv documentation can be found [here](https://pipenv.pypa.io/en/latest/) if you want to know more.

### A little about Pipenv
Pipenv generates two files: Pipfile and Pipfile.lock. Both of these files need to be included in our git repos.

The Pipfile is a file that tracks all of our dependencies for a repo with broad versioning. Most of our dependencies will have an `*` which indicates that we are using the latest version of a package. Some packages may also indicate that we are only using a version before/after some specific version.

The Pipfile.lock file is a file that tracks the specific packages that are installed in the environment (based on what is in the Pipfile). The Pipfile.lock file is not meant to be edited.

### How we use Pipenv
It is recommended by Pipenv and others to always set up you environment by installing from the Pipfile.lock. We don't do it this way, so forget what you just read.

We always build out docker images by installing Pipenv and then running `pipenv intall --skip-lock`. This command will create a virtual environment with the most up-to-date versions of a repos dependencies allowed by the Pipfile. The benefit to this is that it ensures our repos are operating on the most up-to-date code as possible. The downside is that sometimes a new release of a package might not be compatible with your code or other dependencies in your repo. This is where the Pipfile.lock comes in handy.

The Pipfile.lock file is our plan to handle any dependency issues. While developing, keep your Pipfile.lock file up to date by running `pipenv update` regularly. If you hit an issue, you can fall back to the Pipfile.lock on main in Github. When you finish developing, make sure your most up to date Pipfile.lock is included in your PR so we are able to recreate the last known stable environment for the repo.


## Repo Layout

### Flat Layout

### src Layout

## Testing
