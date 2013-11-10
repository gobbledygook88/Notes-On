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

Commands can be written and executed in the Python interpretor, or placed in a program file with the file extension of `.py`. Double-clicking on the Python file will then run it. Generally, lines starting with, and immediately following, `>>>` are written in the interpretor.

First key point to remember is that Python does not need semi-colons at the end of expressions, and that whitespace is very important! Be sure to indent text when required, especially in statements and functions.

### Comments
Start single line comments with the hash key `#`. For multi-line comments use three consecutive double quotes, i.e.

    """ This is a 
    multi-line comment """

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
    >>> print "This is the number " + str(10)      # Concatenate a string and a number with the str() function
    This is the number 10
    >>> print "This is the number " + `10`         # Use backticks instead of the str() function
    This is the number 10

The `str()` function performs _explicit string conversion_, so _explicitly_ converts anything that isn't a string, into a string. _Implicit_ string conversion is literally putting quotes around a sequence of characters.

Another function is `len()` which returns the length of a sequence of characters. Other basic string-specific methods are `.upper()`, `.lower()`, noting that these use dot notation.

    >>> a = "Hello"
    >>> b = "World"
    >>> len(a)
    5
    >>> a + b
    "HelloWorld"
    >>> a.upper()
    "HELLO"
    >>> a.startswith("Hell")
    True
    >>> a.replace("H","M")
    "Mello"

Remember that strings in Python are _immutable_ meaning that you cannot change them once they have been created. Strings can be formatted with `%` via the following syntax

    print "%s" % (variable)

where `s` represents a string. For example,

    print "My name is: %s, and likes %s" % (name, likes)

where `name` and `like` are variables of string type.

### Conversions

Methods exist to convert variables between datatypes, e.g. `int()`, `float()`, and `str()`.

### Raw Input
To ask for user input use the following commands:

    first_input  = input("Your name please:")                       # Just for strings
    second_input = raw_input("Give me something, anything:")        # Any data type is accepted

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

### Loops

There are two kep looping constructs

A while loop:

    n = 10
    while n > 10:
        print 'T-minus', n
        n = n - 1
    print 'Blastoff!'

and a for loop:

    for i in range(5):
        print i

    names = ['Dave','Paula','Thomas','Lewis']
    for name in names:
        print name

### Printing

To print to stdout, we use the `print` command (Python 2)

    print x
    print x, y, z
    print "Your name is", name
    print x,                                # Omits newline

or the print function (Python 3)

    print(x)
    print(x,y,z)
    print("Your name is", name)
    print(x,end=' ')                        # Omits newline

### Files

Opening a file

    f = open("foo.txt","r")                 # Open for reading
    f = open("bar.txt","w")                 # Open for writing

To read data

    data = f.read()                         # Read all data

To write text to a file

    g.write("some text\n")

Reading a file one line at a time

    f = open("foo.txt","r")
    for line in f:
        # Process the line
        ...
    f.close()

### Functions
Generally, there are three parts to a function in Python

    # This multi-line comment
    # is entirely optional, but describes
    # what the function does
    def name_of_function():                 # This is the Header
        # Do something                      # This is the Body

To call the function, just type `name_of_function()`. Parameters may be placed, as a comma separated list, in the parentheses. 

Use _splat arguments_ if you do not know how many arguments a function will take:

    def many_arguments(*args):
        # Do something

    many_arguments("arg1", "arg2", "arg3")

To specific an optional argument, or provide a default value for a parameter, define it in the function header:

    def function_name(req_arg,opt_arg="optional"):
        # Do something

### Data Structures

#### Tuples

A tuple is a read-only collection of related values grouped together

    tuple = ('123', 456, 789)

You can then unpack the tuple in variables

    a, b, c = tuple
    # a = '123'
    # b = 456
    # c = 789

#### Dictionaries

A dictionary is a collection of values indexed by 'keys'

    dict = {
        'alpha' = '123',
        'beta'  = 456,
        'gamma' = 789
    }

    >>> dict['alpha']
    '123'
    >>> dict['beta'] = 1011

#### Lists

A list is an ordered sequence of items (usually of the same type. Python does not care, but we do not normally do this.)

    names = ['Dave', 'Paula', 'Thomas']

    >>> len(names)
    3
    >>> names.append('Lewis')
    >>> names
    ['Dave', 'Paula', 'Thomas', 'Lewis']    
    >>> names[0]
    'Dave'

We can create a new list by applying an operation to each element of a sequence. This is called _list comprehensions_

    >>> a = [1,2,3,4,5]
    >>> b = [2*x for x in a]
    >>> b 
    [2,4,6,8,10]

#### Sets

A set is an unordered collection of unique items, useful for detecting duplicates or related tasks.

    ids = set(['123','456','789'])

    >>> ids.add('101')
    >>> ids.remove('123')
    >>> '456' in ids
    True

### Modules
A module is a file that contains definitions — including variables and functions — that you can use. You can import modules by using the `import` command, such as for `math` library

#### Math

    >>> import math

and call a function such as `math.sqrt()`, where the `math.` prefix tells python to look for the function `sqrt()` in the `math` module.

Note that in your program, you can assign a custom name to a function. For example,
    
    >>> mysqrt = math.sqrt
    >>> mysqrt(9)               # Use new custom name for function
    3.0

You can also just get a single function from a module by _function import_ such as just the `sqrt` function

    from math import sqrt

You can then call the function by `sqrt()`, noting that we do not need the `math.` prefix.

To import everything from a module use an asterisk

    from math import *

but be careful with this. You have been warned.

#### URLs

To read data from the web, we can import the `urllib` module.

    import urllib                           # urllib.request on Python 3

    u = urllib.urlopen("http://www.python.org")
    data = u.read()

### Parsing XML

Parsing a document into a tree
    
    from xml.etree.ElementTree import parse

    doc = parse('data.xml')

Useful methods include `findall('elname')`, `findtext('elname')`

<!-- ## Web Development
packages, frameworks, how to build web application with python, tools required, best practices, --> 

<!-- ## Computer Science
web crawling, data structures -->

<!-- ## Notes -->

## Tips & Tricks

- Use the `-i` flag to enter an interactive session after running a Pythoon script file. Useful when debugging. i.e. `python -i helloworld.py`
- Counter objects for simplified tabulation
    
        from collections import Counter

        words = ['yes','but','no','but','yes']
        wordcounts = Counter(words)

        >>> wordcounts['yes']
        2
        >>> wordcounts.most_common()
        [('yes',2),('but',2),('no',1)]

- Advanced sorting with key-functions. Lambdas, essentially, create small inline functions; the result of key-function determines sort order

        records.sort(key=lambda p: p['COMPLETION DATA'])
        records.sort(ley=lambda p: p['ZIP'])

- Key-functions can also be used to iterate over groups of sorted data

        records.sort(key=lambda r: r['ZIP'])

        from itertools import groupby
        groups = groupby(records, key=lambda r: r['ZIP'])
        for zipcode, group in groups:
            for r in group:
                # All records with same zip-code
                ...

- Building indices to data, which builds a dictionary

        from collections import defaultdict

        zip_index = defaultdict(list)
        for r in records:
            zip_index[r['ZIP']].append(r)

        zip_index = {
            '60640': [ rec, rec, ... ],
            '60637': [ rec, rec, rec, ... ],
            ...
        }

- There are a lot of third-party libraries which provide many out-of-the-box functionality. e.g.

    - numpy/scipy (array processing)
    - matplotlib (plotting)
    - pandas (statistics, data analysis)
    - requests (interacting with APIs)
    - ipython (better interactive shell)
    
## Bibliography & Resources

- [The official Python documentation](http://docs.python.org/)
- [The New Boston](http://www.youtube.com/playlist?list=PLEA1FEF17E1E5C0DA) video tutorials
- [Interactive server-side Python shell](http://shell.appspot.com/) for Google App Engine
- [CodeAcademy](http://www.codecademy.com/tracks/python) interactive Python course
- [Learn Python Through Public Data Hacking](http://www.youtube.com/watch?v=RrPZza_vZ3w)
