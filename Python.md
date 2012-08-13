# Notes on: Python

## Introduction
The following document is a record of useful pointers and tips as I learn the Python programming language. The style is concise and straight to the point, with many basic aspects missing. This is meant for programmers who know another language, so to act as a tip sheet for future reference. There is an emphasis on syntax, differences and best practices.

A variety of sources are, and will be used, as I continually update this document. See the Bibliography section for a full list.

    Beware! If you are a seasoned programmer in Python, quickly `cmd + w` your way out of here. 
    I'm very sure you will find lots of mistakes and inaccuracies, things which aren't strictly true, 
    or are just plain wrong. Save yourselves!

## Installation
Head to [the official Python website](http://www.python.org/download/) and download the relevant installation package for your system.
There are usually two production versions available. Either is fine.

## Core structure

Commands can be written and executed in the IDLE GUI, or placed in a program file with the file extension of `.py`. Double-clicking on the Python file will then run it. Generally, lines starting with, and immediately following, `>>>` are written in the IDLE GUI.

First key point to remember is that Python does not need semi-colons at the end of expressions, and that whitespace is very important! Be sure to indent text when required, especially in statements and functions.

### Numbers and Maths
Numbers (integers or floats) behave similar to other programming languages, though take care with integer division. For example,

    >>> 12/3
    4                           // Correct
    >>> 13/3
    4                           // Incorrect
    >>> 13.0/3.0
    4.333333333333333           // Correct    or    13.0/3    or    13/3.0    or    13/3.

Mathematical operators are the usual `+ - * / %`, with `**` to denote the exponential operator, such as 

    >>> 2 ** 3
    8

### Variables
Python variables are loosely-typed and case-sensitive. Be sure to capitalize the first letter of your booleans.

### Strings
Use the `+` key to concatenate strings, or string literals. The `print` command prints strings:

    >>> print "This is a string"
    This is a string
    >>> print "This is the number " + str(10)      // Concatenate a string and a number with the str() function
    This is the number 10
    >>> print "This is the number " + `10`         // Use backticks instead of the str() function
    This is the number 10

The `str()` function performs _explicit string conversion_, so _explicitly_ converts anything that isn't a string, into a string. _Implicit_ string conversion is literally putting quotes around a sequence of characters.

Another function is `len()` which returns the length of a sequence of characters. Other basic string-specific methods are `.upper()`, `.lower()`, noting that these use dot notation.

Remember that strings in Python are _immutable_ meaning that you cannot change them once they have been created. Strings can be formatter with `%` via the following syntax

    print "%s" % (variable)

where `s` represents a string. For example,

    print "My name is: %s, and like %s" % (name, like)

where `name` and `like` are variables of string type.

### Comments
Start single line comments with the hash key `#`. For multi-line comments use three consecutive double quotes, i.e.

    """ This is a 
    multi-line comment """

### Raw Input

### Control Flow
Syntax for conditional statements are

    if condition1:
        # Do something
    elif condition2:
        # Do something else
    else:
        # Do another thing if condition1 and condition2 both fail

_Note: Remember the colons!_

There are six comparators: `==`, `!=`, `<`, `<=`, `>`, and `>=`.

Boolean operators are: `and`, `or`, and `not`, and here are the truth tables (just for kicks)

                and                                  or                            not
    -------------------------------     -------------------------------     ------------------
    True    |   True    |   True        True    |   True    |   True        True    |   False 
    True    |   False   |   False       True    |   False   |   True        False   |   True
    False   |   True    |   False       False   |   True    |   True
    False   |   False   |   False       False   |   False   |   False

### Functions
Generally, there are three parts to a function in Python

    # This multi-line comment
    # is entirely optional, but describes
    # what the function does
    def name_of_function():                 // This is the Header
        # Do something                      // This is the Body

To call the function, just type `name_of_function()`. Parameters may be placed, as a comma separated list, in the parentheses. 

Use _splat arguments_ if you do not know how many arguments a function will take:

    def many_arguments(*args):
        # Do something

    man_arguments("arg1", "arg2", "arg3")

### Modules
A module is a file that contains definitions — including variables and functions — that you can use. You can import modules by using the `import` command, such as for `math` library

    >>> import math

and call a function such as `math.sqrt()`, where the `math.` prefix tells python to look for the function `sqrt()` in the `math` module.

Note that in your program, you can assign a custom name to a function. For example,
    
    >>> mysqrt = math.sqrt
    >>> mysqrt(9)               // Use new custom name for function
    3.0

You can also just get a single function from a module by _function import_ such as just the `sqrt` function

    from math import sqrt

You can then call the function by `sqrt()`, noting that we do not need the `math.` prefix.

To import everything from a module use an asterisk

    from math import *

but be careful with this. You have been warned.

<!-- ## Web Development
packages, frameworks, how to build web application with python, tools required, best practices, --> 

<!-- ## Computer Science
web crawling, data structures -->

<!-- ## Notes -->

## Bibliography & Resources

- [The official Python documentation](http://docs.python.org/)
- [The New Boston](http://www.youtube.com/playlist?list=PLEA1FEF17E1E5C0DA) video tutorials
- [Interactive server-side Python shell](http://shell.appspot.com/) for Google App Engine
- [CodeAcademy](http://www.codecademy.com/tracks/python) interactive Python course