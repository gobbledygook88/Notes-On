# Notes on: Objective-C

## Introduction
The following document is a record of useful pointers and tips as I learn Objective-C. The style is concise and straight to the point, with many basic aspects missing. This is meant for programmers who know another language, so to act as a tip sheet for future reference. There is an emphasis on syntax, differences and best practices.

    Beware! If you are a seasoned programmer in Objective-C, quickly `cmd + w` your way out of here. 
    I'm very sure you will find lots of mistakes and inaccuracies, things which aren't strictly true, 
    or are just plain wrong. Save yourselves!

## Basics
Objective-C is a super-set of the C language. Any C code is valid Objective-C code. Objective-C provides Object-Oriented capabilities to C. We will be discussing syntax in the rest of this document, rather than the core OOP principles.

Some examples will make use of Apple's Cocoa framework, mostly the `foundations` library.

### Other important notes
Use the `#import` directive, rather than `#include` directive to include Objective-C libraries.

Instead of the usual `stdlib`, etc, C libraries, use the `Foundations` library which is part of the Apple Cocoa framework.

    #import <Foundations/Foundations.h>

The `Foundations` library provides the `NSLog()` method, which performs the same role as `printf()` in C. The syntax is identical.

String literals in Objective-C are prefixed by the `@` symbol. So coupled with the `NSLog()` method, we get

    NSLog(@"This line will be printed to console.");

An additional loop structure is provided by Objective-C. This is the `for-in` loop. This is referred to as the _fast-enumeration_ syntax which is more efficient than the standard `for`/`while` loops when used with Objective-C Collections.

    NSArray *models = @[@"Ford", @"Honda", @"Nissan", @"Porsche"];
    for (id model in models) {
        NSLog(@"%@", model);
    }

All Objective-C objects are referenced as pointers. The syntax therefore works naturally with pointers.

The macro `nil` is provided in Objective-C to represent null objects. It is identical to the C `NULL` macro. Though it is good practice to use `nil` for Objective-C objects, and `NULL` for C pointers.

## Classes

### Methods
To call a method in Objective-C, we pass `messages`. Use the schema

    [ClassInstance Message]

### Interface and Implementation
Objective-C separates __interface__ from __implementation__. Most classes are structured so that there is an implementation file (`.m`), and an interface file (`.h`).

#### Interface 
The basic syntax for declaring a class is

    // Filename: Class.h
    @interface ClassName : InheritedClass {
        // Attributes
    }

    // Method definitions
    - (returnType) methodName: (paramType) paramName;
    - (returnType) first:     (firstparamType)  firstParamName
                   andSecond: (secondParamType) secondParamName;
    @end

The `-` means this method is an instance method. To declare a class method use a `+`. Most Objective-C names are given as plain english.

#### Implementation
The basic syntax for an implementation file is

    // Filename: Class.m
    #import "Class.h"
    @implementation ClassName
    - (returnType) methodName: (paramType) paramName {

    }
    @end

## Resources

- [MobileTuts+](http://mobile.tutsplus.com/series/learn-objective-c/)
- [RyPress](http://rypress.com/tutorials/objective-c)
- [Mac Developer Library](http://developer.apple.com/library/mac/navigation/)