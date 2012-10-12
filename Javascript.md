# Notes on: Javascript

## Introduction
The following document is a record of useful pointers and tips as I learn Javscript. The style is concise and straight to the point, with many basic aspects missing. This is meant for programmers who know another language, so to act as a tip sheet for future reference. There is an emphasis on syntax, differences and best practices.

    Beware! If you are a seasoned programmer in Javascript, quickly `cmd + w` your way out of here. 
    I'm very sure you will find lots of mistakes and inaccuracies, things which aren't strictly true, 
    or are just plain wrong. Save yourselves!

## Basics
Javascript is awesome. Variables are loosely-typed. Curly braces (most of the time) and semi-colons are essential. Things just work like you expect them to ... or do they?

### Functions
There are two ways of declaring a function. Both are valid, but try to use the second method as it keeps the usage of `var` to a minimum. Functions can be placed in any order, anywhere in your script file as they are "..." (there is a word for this property ... and I forgot!).

    // Method One
    var function_name = function(arg_one,arg_two) {
        # Do something
    };

    // Method Two (recommended)
    function function_name(arg_one,arg_two) {
        # Do something
    }

Call the function such as `function_name(prop_one,prop_two)`.

### Objects
There are two ways to declare an object in Javascript: through _literal notation_ or via a _constructor_.

    // Literal Notation
    var object_one = {
        prop_one: "value one",
        prop_two: "value two"
    };

    // Object Constructor
    var object_two = new Object();
        object_two.prop_one = "value one";
        object_two.prop_two = "value two";

To refer to an object property, there are two notations

    // Dot Notation
    var prop1 = object_one.prop_one;

    // Bracket Notation
    var prop2 = object_one["prop_one"];

Methods are functions that are associated with a particular object.

### Constructors & Classes
We have already met the `Object` constructor, here are some more alongside their respective literal notation

    // Object Constructor               |       // Literal Notation
    var obj = new Object();             |       var obj = {};

    // Array Constructor                |       // Literal Notation
    var arr = new Array();              |       var arr = [];

    // String Constructor               |       // Literal Notation
    var str = new String();             |       var str = "";

To create your out constructor, use the following

    function cons_name(arg_one,arg_two) {
        this.prop_one = arg_one;
        this.prop_two = arg_two;
    }

When defining your own constructor, you are in fact defining a new _class_.

### Types
As we have seen there are various types of variables. Use the `typeof` command to return a variable type, such as

    var anObject = { prop: "value" };
    var aNumber  = 1;
    var aString  = "string";

    console.log( typeof anObject );     // object
    console.log( typeof aNumber );      // number
    console.log( typeof aString );      // string

### Object Methods
Objects in Javascript have many methods attached to them natively. Say we have an object called `obj`

    obj.hasOwnProperty("prop_name");        // Returns true if the object has that particular property

### Prototypes
A prototype is an object from which other objects inherit properties.

In general, if you want to add a method to a class such that all members of the class can use it, we use the following syntax to _extend the prototype_

    class.prototype.method_name = function(args) {
        // Do something
    }

Note that we can even extend the prototype of a built-in javascript class.

### Inheritance
You can create new classes which are _children_ of other classes, which effectively become the _parents_.

    // The child _inherits_ properties and methods from the Parent class
    Child.prototype = new Parent();

Hence, you can create _prototype chains_, whereby children and grandchildren etc can access properties and methods belonging to their parents and grandparents etc.

To test whether a child is an instance of their parent, we can use the `instanceof` keyword.

    console.log(Child instanceof Parent);

### Encapsulation
Encapsulation is the grouping of an object's data together with its methods. Encapsulating objects allows us to reuse blocks of code to have a more efficient program.

### Public and Private variables
Up to now, we have been defining _public_ variables in our classes and objects.

    function Class_Name(arg1,arg2) {
        this.arg1 = arg1;               // Public
        this.arg2 = arg2;               // Public
        var arg3 = "something";         // Private
    }

To access a private variable, we can define a public method, a _getter_.

    function Class_Name(arg1,arg2) {
        this.arg1 = arg1;               // Public
        this.arg2 = arg2;               // Public
        var arg3 = "something";         // Private
        this.getArg3 = function() {
            return arg3;
        }
    }

Methods can also be private. Just create a public method that returns the private method.

### Loops
There are various loops in Javascript, as in many other programming languages.

    var i,                          // Iterator
        NUM = 100;                  // Arbitrary limit

    for( i=0; i<NUM; i++ ) {        // A `for` loop
        // Do something
    }

    for( var thing in things ) {    // A `for-in` loop
        // Do something
    }

### Arrays
An _indexed array_ is just an ordered list.

    var array = ["string","another string","yet another string"];

To access the array, we use literal notation with an index based at zero.

    console.log(array[0]);                  // string

To add to the end of an array, we use the `push()` method.

    array.push("one more string");          // ["string","another string","yet another string","one more string"]

The `splice()` method can be used to remove items from an array. It takes two arguments, both integers, specifying the index to start at, and the number of items to remove.

    array.splice(1,2);                      // ["string","one more string"]

To copy an array, or part of an array, we use the `slice()` method. If there are no arguments, then the whole array is copied. Else, we state two arguments which specify the start and end indexes required.

    array.slice();                          // ["string","another string","yet another string","one more string"]
    array.slice(1,2);                       // ["another string","yet another string"]

Arrays can contain any objects, in any combination, even arrays themselves to make multi-dimentional arrays.

## Web Development

### DOM Scripting

## Resources

- [CodeAcademy](http://www.codecademy.com/) courses and [Glossary](http://www.codecademy.com/glossary/javascript)
- [Javascript Weblog](http://javascriptweblog.wordpress.com/2010/06/07/understanding-javascript-prototypes/) on Prototypes
- [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/JavaScript) Javascript documentation
- [Javascript Scope Quiz](http://madebyknight.com/javascript-scope/)
- [Understanding Javascript OOP](http://killdream.github.com/blog/2011/10/understanding-javascript-oop/)