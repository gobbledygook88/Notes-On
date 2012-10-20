# Notes On: C++

## Introduction
The following document is a record of useful pointers and tips as I learn the C++ programming language. The style is concise and straight to the point, with many basic aspects missing. This is meant for programmers who know another language, so to act as a tip sheet for future reference. There is an emphasis on syntax, differences and best practices.

A variety of sources are, and will be used, as I continually update this document. See the Bibliography section for a full list.

    Beware! If you are a seasoned programmer in C++, quickly `cmd + w` your way out of here. 
    I'm very sure you will find lots of mistakes and inaccuracies, things which aren't strictly true, 
    or are just plain wrong. Save yourselves!

## Installation
We will be using the `g++` compiler throughout

## Basics

### Starting up
The most basic program written in C is shown below, with annotations:

    #include <iostream>                                     // Include input/output library

    int main() {                                            // `main` function declaration and block
        std::cout << "Hello world!" << std::endl;           // Print to terminal with streams
        return 0;                                           // Return statement
    }

To compile the program, we use the following commands in terminal:

    > g++ hello.cc -o hello
    > ./hello

Here is a quick run-down of the syntax:

- C++ is a high level, procedural, compiled programming language
- Variables are strictly typed, meaning you must specify the type of variable
- Organise methods into a series of blocks
- Must use semi-colons at the end of all statements
- Code is case sensitive

### Comments
To comment code, i.e. blocks of text which are to be ignored by the compiler, use

    // Two forward-slashes for inline, single-line comments

or, alternatively

    /*
     *  Multi-line comment. 
     */

Get into the habit of commenting your code while you write it. Just do it!

### Variables 
Variables are case-sensitive. A variable declaration looks like:

    <datatype> <identifier> [= <value>]

- The `<datatype>` specifies what type of variable it is, and is one of `int`, `float`, `double`, `char`, `long`, `bool`.
- `<identifier>` is the name of the variable
- `[= <value>]` is an optional parameter which can set a value to the variable

### Strings
To use strings in your code, use the following syntax:

    std::string this_is_a_string = "I am the string";

Be sure to include the `<string>` library in the head of your file.

### Arrays
Use arrays to store data of the same type. Use the schema:

    <datatype> <identifier>[ <size> ];

Remember that indexes in arrays start at zero. The size of the array must be fixed at compile time.

To set a value into an array:

    int array[5];

    for( int i = 0; i < 5; i++ ) {
        array[i] = i;                                   // [0,1,2,3,4]
    }

To get a value from an array:

    std::cout << array[0] << std::endl;                 // 0

### Operators
There are two types of operators:

- Binary operator: acts on two variables
- Unary operator: acts on a single variable

The main operators are:

- Arithmetic: `+ - * / %`
- Assignment: `=`
- Compound Assignment: `+= -= *= /=`
- Increment/Decrement: `++ --`
- Relational: `== != > < >= <=`
- Logical: `! && ||`

Take note that `double half = 1/2;` returns zero as it is performing integer arithmetic. When defining a `double`, use a decimal point: `double half = 1.0/2.0;`.

### Printing to terminal
We use the stream operators `<<` and `>>` to pass and receive data from the terminal. To print to the terminal, use:

    std::cout << "This text will be printed to terminal. ";
    std::cout << "I'm printed on the same line!";
    std::cout << std::endl;                                         // End line

To read input from the terminal, use `std::cin`:

    int a;
    std::cout << "Enter an integer: ";
    std::cin >> a;

When printing to the terminal, it is good practice to format your output into columns. To do this, use the `std::cout.width()` command, for example:

    double pi   = 3.141592654;
    double log2 = 0.301029996;

    std::cout.precision(5);                 // Set output precision for numbers
    
    std::cout.width(20);
    std::cout << "pi =";
    std::cout.width(10);
    std::cout << pi << std::endl;

Will produce:

                    pi =     3.1415
                  log2 =     0.3010

### Command Line Arguments
To read command line arguments, passed to the executable via the terminal, add extra parameters to the `main()` function:

    int main( int argc, char *argv[] ) {
        std::cout << "This program was called with \"" << argv[0] << "\"" << std::endl;

        if( argc > 1 ) {
            for( int count = 1; count < argc; count++ ) {
                std::cout << "argv[" << count << "] = " << argv[count] << std::endl;
            }
        } else {
            std::cout << "The command has no other arguments" << std::endl;
        }
    }

### Conditional Structures
The usual conditional structures are available in C++

`if` statement:

    if( condition ) {
        // Do something
    } else if( another condition ) {
        // Do something else
    } else {
        // Do another thing
    }

`switch` statement:

    switch( identifier ) {
        case value1:
            statement1;
        break;

        case value2:
            statement2;
        break;

        case value3:
            statement3;
        break;

        // Add as many cases as required ...

        default:
            defaultstatments;
    }

### Iterative Structures
Again, the usual iterative structures are available.

`for` loop:

    for( initialiser; condition; update ) {
        // Do something over and over
    }

`while` loop:

    while( condition ) {
        // Do something
    }

`do-while` loop:

    do {
        // Do something at least once
    } while( condition );

### Functions
The schema for a function is as follows:

    <returntype> <name> ([<datatype> <identifier>[,...]]) {
        // Do something
    }

Be sure to prototype the function if your function definitions are positioned after the `main()` function.

The additional datatype `void` can be given to a function if it does not return anything.

Place an ampersand `&` before a parameter name of a function to pass the value by reference, rather than value. More on this in the Pointers section later.

Note that arrays passed to functions are passed by reference by default.

### Dynamic Arrays
Dynamic arrays allocate memory for an array at run-time, rather than compile time. Use the schema:

    <datatype> *<identifier> = new <datatype>[ <size> ];

Note that the size does not have to be specified at compile time. Be sure to check that allocation of memory was successful, and delete the array when it is no longer required.

    int N;
    std::cout << "Enter the size of the array: ";
    std::cin >> N;

    double *array = new double[ N ];

    if( !array ) {
        std::cout << "Could not allocate memory for array" << std::endl;
    }

    delete[] array;

### File Input and Output
There are several functions available in the `<fstream>` library which allow for reading and writing text files.

- `std::ifstream` - file input
- `std::ofstream` - file output
- `std::fstream` - both file input and output

The following code gives an example on how to write to a file:

    #include <fstream>

    ...

    std::ofstream outputFile;                                   // Declare file stream
    outputFile.open( "output.txt" );                            // Open file

    if( !outputFile.good() ) {                                  // Check if file is good
        std::cout << "Can not open file" << std::endl;
    } else {
        outputFile << "Line 1" << std::endl;
        outputFile << "Line 2" << std::endl;
    }
    
    outputFile.close();                                         // Close file

The following code gives an example on how to read a file:

    #include <fstream>
    #include <string>

    ...

    std::ifstream inputFile;                                    // Declare file stream
    inputFile.open( "input.txt" );                              // Open file

    if( !inputFile.good() ) {                                   // Check if file is good
        std::cout << "Can not open file" << std::endl;
    } else {
        while( !inputFile.eof() ) {                             // Run through file until End of File
            std::string line;                                   // Declare line string
            getline( inputFile, line );                         // Write the line to variable
            std::cout << line << std::endl;                     // Print out line to terminal
        }
    }

    inputFile.close();                                          // Close file

There are various file access modes which can be specified to ensure files are used properly. Seperate multiple modes with the pipe `|` symbol.

- `std::ios::in` - file is for input only
- `std::ios::out` - file is for output only
- `std::ios::binary` - file is formatted as binary
- `std::ios::ate` - file is opened with the initial position at the end of the file
- `std::ios::app` - append data to the end of the file if it already exists
- `std::ios::trunc` - delete the file and start afresh if the file already exists

## STL Classes
STL stands for Standard Template Library, and is a collection of templated containers and associated algorithms for manipulating those containers. Each container is a C++ class. For example, the `<vector>` (templated) class:

    #include <vector>

    ...

    std::vector< double > aVector;                              // Empty vector

Instances of the `<vector>` object can be created in various ways, including the example above. Others include:

    std::vector< double > bVector( size );                  // Vector of size `size`
    std::vector< double > cVector( size, 1.0 );             // Set a value on creation
    std::vector< double > dVector( bVector );               // Copy a vector

The `<vector>` class has many member functions:

    std::vector< double > aVector( 5 );

    aVector.size();                                         // Returns the size of the vector
    aVector.push_back( 7.0 );                               // Add an element to the vector
    aVector.pop_back();                                     // Remove last element of vector
    aVector.resize( 10 );                                   // Resize vector
    aVector.clear();                                        // Clear the vector completely

### Iterators
An iterators is an object which marks a position in a container object and allows such a container to be traversed. Each STL container can use iterators to access its contents. An iterator is specific to the type of container it is associated with. An iterator for a `std::vector< double >` would be declared as 

    std::vector< double >::iterator it;

They can be dereferenced to access the value at the position they mark, either as

    std::cout << *it << std::endl;

or

    double &value = *it;
    std::cout << value << std::endl;

The `begin()` and `end()` member functions return an iterator marking the beginning and end of the vector, respectively.
There are several member functions to note:

    it.begin();                                             // Return iterator marking the beginning of the vector
    it.end();                                               // Return iterator marking the end of the vector
    aVector.insert( it, 7.0 );                              // Insert value at position of iterator 
    aVector.erase( it );                                    // Erase value at position of iterator

### Templates
Templates allows for generalisation of routines or classes to many data types, usually reducing code size.

    template< typename T >
    void f( T x, T y );

The `T` is an alias for a datatype which is specified when we call the routine:

    f< int >( x, y );

## Classes and Objects
A _class_ is a programming construct which encapsulates and manipulates data. An instance of a class is called an _object_ which has real memory allocated to it.

### Encapsulation
Encapsulation can be achieved via various _access specifiers_:

- Public
- Private
- Protected

### Structures
All members are publicly accessible from outside the class.

    struct Person {
        std::string name;
        std::string address;
        unsigned int age; 
    };

    ...

    Person me; 
    me.name = "Tom";
    
    Person other = me;                                      // Copies everything from me into other

### Classes
Use the `class` keyword and place statements within curly braces. Remember to put a semi-colon at the end of the definition! For example:

    class Point {
        public:
            double getX();
            double getY();
            void setX( double val ); 
            void setY( double val );
            double norm();

        private: 
            double x_; 
            double y_;
    };

By setting member variables to be private, and provide public getter and setter methods, we can validate the input to ensure data integrity. Member functions are defined outside the class construct, and we precede them with the class name and scope operator.

    void Point::setX( double val ) {
        x_ = val; 
    }

    void Point::setY( double val ) {
        y_ = val; 
    }
    
    double Point::getX () {
        return x_; 
    }

    double Point::getY () {
        return y_;
    }
    
    double Point::norm () {
        return std::sqrt( x_ * x_ + y_ * y_ ); 
    }

### Constructors
A constructor is called when a class is first instantiated, always has the same name as the class, and has a return type of `void`. Constructors may, for example, initialise data members to a suitable initial value, and/or allocate any dynamic memory.

There are two special constructors:

- Default constructor: takes no parameters
- Copy constructor: takes a constant reference to an instance of the class

    class Point {
        public:
            Point();                                                        // default constructor
            Point( double x, double y );                    
            Point( const Point& source );                                   // copy constructor
            ~Point();                                                       // destructor
    
            double getX();
            ... 
    }

### Initialisation Lists
Initialisation lists are a more efficient way of setting the starting values for member variables in an object.

    Point::Point( double x, double y ) : x_( x ), y_( y ) {}

### Accessing Member Functions
Use the dot operator to call and access member functions of a class. For example,

    int main () {
        Point a; 

        a.setX( 1.1 ); 
        a.setY( 2.2 );

        Point c = a;

        std::cout << "(" << c.getX() << ", " << c.getY() << ")" << std::endl; 

        Point b( 3.2, 5.6 );
        std::cout << "distance from origin: " << c.norm() << std::endl;
    }

### Inheritance
Inheritance allows the creation of classes which are derived from other classes – they become a specialisation of another class. The specialised class is called the _derived class_. The class from which it inherits is called the _base class_. 

A derived class _extends_ the functionality of a base class. All members of the base class which are public or protected are available in the derived class. Additional members can be added to the derived class to extend the functionality of the base class.

    class BaseClass { ... };

    class DerivedClass : public BaseClass { ... };

Note that the access specifier can only be `public` or `private`. The constructor of the derived class can only initialise
members which the derived class can access. A derived class could never initialise private members of the
base class. Thus, we must use the constructor of the base class to initialise those members.

## Operator Overloading
Operator overloading is the definition of standard operators in the context of a class. Depending on the function of the class, most operators can be overloaded so that they make sense for the class. Such as overloading the `+` operator for the Vector class to perform component-wise vector addition. Use the following schema to overload an operator:

    <returntype> <class>::operator<sign> ({<operand>}) {}

- `<returntype>` is usually `<class>`, `bool` or `void`
- `<class>` is the name of the class
- `operator<sign>` specifies which operator is being overloaded
- `<operand>` specifies the second operand in the case of binary operators

For example, here is how to overload the `==` operator to test if two `Point`s are equal

    class Point {
        bool operator==( const Point &source ) {
            if( ( x_ == source.x_ ) && ( y_ == source.y_ ) ) {
                return true;
            }
            return false;
        }
    };

- The parameter is the second operand of the comparison
- It should be passes as a constant reference as we don't want to change the RHS

Then to use this overloaded operator:

    Point a, b;
    ...
    if( a == b ) { ... }

When overloading the assignment `=` operator ensure to return a reference to the original object:

    Point& Point::operator=( const Point &source ) {
        x_ = source.x_;
        y_ = source.y_;
        return *this;
    }

This means we can do this:

    Point a, b, c, d;
    a = b = c = d;

### Self-Assignment
Be careful, when overloading the assignment operator, not to perform self-assignment. Check first that the memory address of the operand `source` is not the same as that of `this`.

### Non-Member Operators
We have seen we can do this:

    bool Complex::operator==( double val );

    Complex a;
    if( a == 5.0 ) { ... }

But, even though this makes sense, we can't do

    if( 5.0 == a ) { ... }

However, we can define an operator on classes _outside_ a class:

    class Complex {
        bool operator==( double value );
    };
    ...
    bool operator==( double value, const Complex &source ) {
        return ( source == value );
    }
    ...
    if( 5.0 == a ) { ... }

## `static` Members
`static` members are variables or routines which are shared among all instances of a class. There is only one unique copy. 

- `static` variables can be thought of as global variables
- A `static` class member cannot be initialised more than once, so it is not initialised in a constructor of a class. It must be initialised outside the class definition

## The `typedef` Keyword
C++ allows the definition of our own types based on other existing data types. Use the schema:

    typedef ExistingType NewTypename;

## The `explicit` Keyword
In C++, the compiler is allowed to make one implicit conversion to resolve the parameters to a function. The compiler can use single parameter constructors to convert from one type to another in order to get the right type for a parameter. Prefixing the `explicit` keyword to the constructor prevents the compiler from using that constructor for implicit conversions.

    class Foo {
        public:
            Foo (int foo) : foo_ (foo) {}
            int GetFoo() {
                return foo_;
            }

        private:
            int foo_;
    };
    
    ...

    void DoBar( Foo foo ) {
        int i = foo.GetFoo();
    }

    ...

    int main() {
        DoBar( 42 );
    }

## Resources

- [MA913 Scientific Computing](http://go.warwick.ac.uk/ma913)