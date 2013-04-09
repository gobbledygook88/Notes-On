iOS Application Development
===========================

## Introduction
The following document is a record of useful pointers and tips as I learn iOS application development. The style is concise and straight to the point, with many basic aspects missing. This is meant for programmers who know another language, so to act as a tip sheet for future reference. There is an emphasis on syntax, differences and best practices.

    Beware! If you are a seasoned iOS developer, quickly `cmd + w` your way out of here. 
    I'm very sure you will find lots of mistakes and inaccuracies, things which aren't strictly true, 
    or are just plain wrong. Save yourselves!

## Basics
Applications for iOS are written in the Objective-C language. Examples are written using XCode. 

Many classes provided via the iOS SDK are prefixed by 'NS', which stands for 'NextStep'. This is because Mac OS X is based on NeXTSTEP, the object oriented language created for NeXT.

## Variable Types
Here are just a few

- `NSString       `: is a string of text that is immutable.
- `NSMutableString`: is a string of text that is mutable.
- `NSArray        `: is an array of objects that is immutable.
- `NSMutableArray `: is an array of objects that is mutable.
- `NSNumber       `: holds a numeric value.

### Example

    #import <Foundation/Foundation.h>  
    int main (int argc, const char * argv[]) {  
        NSString *testString;                                           // Declare pointer 
        testString = [[NSString alloc] init];                           // Allocate memory
        testString = @"Here's a test string in testString!";            // Define string
        NSLog(@"testString: %@", testString);                           // Print string
        return 0;  
    }

    // Alternatively, to allocate memory 
    // testString = [NSString alloc];
    // [testString init];

The method `init` initialises all the instance variables of the class. The `%@` represents an Objective-C object.

A brief sidetrack to inheritance. The hierarchy for the above classes are as follow

    NSObject
    |-- NSString
    | \- NSMutableString
    |-- NSArray
    | \- NSMutableArray
    \-- NSValue
      \- NSNumber

The `init` method for `NSObject` simply returns `self`.

## Cocoa
To include Cocoa use the following

    // Filename: Class.h
    #import <Cocoa/Cocoa.h>
    @interface ClassName : InheritedClass {
        // Attributes
    }

    // Method definitions
    - (returnType) methodName;
    @end
    </cocoa>

## Resources

- [MobileTuts+](http://mobile.tutsplus.com/series/learn-objective-c/)
- [Mac Developer Library](http://developer.apple.com/library/mac/navigation/)