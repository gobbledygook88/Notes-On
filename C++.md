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

To compile the program, we use the following commands in termainl:

    > g++ hello.cc -o hello
    > ./hello

Here is a quick run-down of the syntax:

- C++ is a high level, procedural, compiled programming language
- Variables are strictly typed, meaning you must specify the type of variable
- Organise methods into a series of blocks
- Must use semi-colons at the end of all statements
- Code is case sensitive
- 

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

    // `if` statement
    if( condition ) {
        // Do something
    } else if( another condition ) {
        // Do something else
    } else {
        // Do another thing
    }

&nbsp;

    // `switch` statement
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

    // `for` loop
    for( initialiser; condition; update ) {
        // Do something over and over
    }

&nbsp;

    // `while` loop
    while( condition ) {
        // Do something
    }

&nbsp;

    // `do-while` loop
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

    it.begin();                                             // Return iterator marking the beginnning of the vector
    it.end();                                               // Return iterator marking the end of the vector
    aVector.insert( it, 7.0 );                              // Insert value at position of iterator 
    aVector.erase( it );                                    // Erase value at position of iterator

### Templates
Templates allows for generalisation of routines or classes to many data types, usually reducing code size.

    template< typename T >
    void f( T x, T y );

The `T` is an alias for a datatype which is specified when we call the routine:

    f< int >( x, y );

## Classes
An instance of a class is called an _object_.

## Resources

- [MA913 Scientific Computing](http://go.warwick.ac.uk/ma913)