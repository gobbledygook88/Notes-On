# Notes on: C

## Introduction
The following document is a record of useful pointers and tips as I learn C. The style is concise and straight to the point, with many basic aspects missing. This is meant for programmers who know another language, so to act as a tip sheet for future reference. There is an emphasis on syntax, differences and best practices.

    Beware! If you are a seasoned programmer in C, quickly `cmd + w` your way out of here. 
    I'm very sure you will find lots of mistakes and inaccuracies, things which aren't strictly true, 
    or are just plain wrong. Save yourselves!

## Installation
We will be using the `gcc` compiler throughout.

### Mac
X-Code

### Windows

## Basics
### Starting up
The most basic code-base for a program written in C is shown below, with annotations:

    #include <stdio.h>                  // Preprocessor Directives and Declarations

    int main() {                        // Initiation function
        printf("Hello World!");         // Function body
        return 0;
    }

To compile and run the program, use the following commands in a terminal

    > gcc hello.c -o hello
    > ./hello

Here is a quick run-down of the syntax:

- Variables are strictly-typed, meaning you must specify the type of the variable.
- So are functions.
- Semi-colons are required at the end of each statement.
- You will probably want to include the `stdio.h` header file in all your programs. 
- Comments are declared with `//` for single-line, and `/* ... */` for multi-line.
- Every function must return something (?) and must be of the same type as what you declared.
- Variables and functions are case-sensitive.

### Variables
The standard types of variables are what you should expect, listed below, except there is no `string` variable type.

- `char` - a Character
- `int` - an Integer (up to &plusmn; 32767 on a 32-bit system)
- `long int` - a larger Integer (up to &plusmn; 2147483647)
- `float` - a Floating Point Number
- `double` - a Floating Point Number with greater precision

To declare a variable, we use the following syntax

    int i;                              // Single variable declaration
    int i,j,k;                          // Multiple variables separated with a comma
    int i = 0;                          // Assign a value when declaring the variable

For strings, we use a character array whose length is one more than te length of the word. This takes into account the newline at the end of the string.

    #include <stdio.h>
    char string[10] = "a string";

Variables declared outside any functions are called _global variables_ and can be accessed by any function in the program. Variables declared inside a function are called _local variables_ and are destroyed once the function exits. A _static variable_ is a local variable but it's value is preserved on exit of a function, and is available for use when the function is next called.

    static int name_of_static_var;

_Constant variables_ are declared in a similar way

    const float pi = 3.1415926;

Alternatively, you can use the C-proprocessor directive `#define`:

    #define pi 3.1415926

### Input & Output
Some standard library functions require a conversion specification for different types of variables.

    %d      // int
    %f      // float or double
    %e      // float or double (in scientific notation)
    %c      // character
    %s      // character string

One function which uses these is `printf`

    #include <stdio.h>

    char str[] = "a string";

    int main() {
        printf("This is %s",str);
        return 0;
    }

Special characters can also be used, such as `\t` and `\n`, to indicate a tab or newline, respectively.

For integers, floats and doubles you can format the output. 

    int x = 5;
    printf("%10.4d",x);             // prints out 0005

The `.` allows for precision. The number `10` puts `0005` over 10 spaces so that the number `5` is on the tenth spacing.

To capture input, from a keyboard for example, we use the `fgets(str,sizeof(str),stdin)` function, which takes three parameters: `str` is a character array, `sizeof(str)` is the maximum number of characters to be read (plus one for the newline), and `stdin` the source of the input (keyboard).

`fgets()` returns the whole string. To remove the newline character use

    str[strlen(str)-1] = "\0";      // Set last character to `null`

To convert the text read in via `fgets()` we use the function `sscanf(line,"%d %d %d",&num1,&num2,&num3)`, where `line` is the input from `fgets()`, and `&num1`, `&num2`, `&num3` are points to already initiated integers.

To read input from a file, rather than a keyboard, just provide the filename in `fgets()`

    #include <stdio.h>

    const char FILE_NAME[] = "inputdata.dat";
          char line[100];
          int  num1, num2, num3;

    int main() {
        FILE *in_file;                                                  // Indicate you want a file
        in_file = fopen(FILE_NAME,"r");                                 // Open the file

        fgets(line,sizeof(line),in_file);                               // Get input
        sscanf(line,"%d %d %d",&num1,&num2,&num3);                      // Convert to numbers
        printf("You inputted numbers: %d %d %d\n",num1,num2,num3);      // Print out input

        return 0;
    }

### Strings
There are several string specific functions which are good to know:

    strcopy(new_str,str);           // Copies the string `str` to `new_str`
    strcat(new_str,str);            // Adds the string `str to `new_str`
    strlen(str);                    // Returns the length of the string `str`

### Operators
The usual mathematical operators are present: `+`, `-`, `/`, `*`, `%`. Note (expecially when using division) that if both operands are integers, then the output will be an integer.

The comparision operators are `==`, `!=`, `<`, `>`, `<=`, `>=`, which return `0` or `1`. Note that when comparing two real numbers it is usually better to use a tolerance:

    same = (abs(a-b) < tol);

Boolean operators are `&&`, `||`, `!`.

It is often useful to know the order of precedence when using many operators in one statement.
    
    high    parentheses         ()
            unary               !, ++, --
            multiplication      *, /, %
            addition            +, -
            comparision         ==, !=, <, >, <=, >=
            and                 &&
            or                  ||
    low     assignment          +=, -=, *=, /=, %=

### Conditionals
_Statement block_ are groups of code within a single set of curly braces `{ }`.

_Conditional blocks_ are similar to other programming languages:

    if( statement1 ) {
        // Do something
    } else if( statement 2 ) {
        // Do something else
    } else {
        // Do another thing
    }

### Loops
The usual loops are present:

    while( statement ) {                // While loop
        // Do something
    }

    do {                                // Do-While loop
        // Something
    } while( statement )

    for(inits; tests; actions) {        // For loop
        // Do something
    }

The keywords `break` and `continue` can be used in loops.

### Arrays
Just like a character array, we can have arrays which hold other types of variables, such as an integer array: `int int_arr[]`. Arrays are zero-index-based. Be careful not to address array elements outside of the declared array, as this will result in errors. Writing to an index outside of the declared array means writing to a random memory slot, and may cause weird things to happen!

Multi-dimensional arrays are declared such as `int mul_arr[][]`. Since memory is linearly addressable, matrices in C are stored in _row-major order_.

### Functions
All functions must be declared, known as a function prototype, before the `main()` function. The general syntax is as follows

    return_type function_name( arg1_type arg1, arg2_type arg2, ... , argn_type argn );

The `return_type` of a function is the variable type that the function returns. If a function doesn't return anything, set `return_type` to `void`.

    #import <stdio.h>

    int product(int a, int b);

    int main() {
        int a = 2;
        int b = 3;

        printf("The product of %d and %d is %d\n", a, b, product(a,b));
        return 0;
    }

    int product(int a, int b) {
        return a * b;
    }

### Pointers
Variables can be accessed, as before, by name. When variables are passed to functions as such, in fact, only its value is passed through as a local variable. Any changes made to an argument will not affect the original variable. If you wish to change the original variable by a function, you will need to pass through a variable _pointer_.

Pointers are objects that _points_ to a memory address of another object. Pointers must be declared (since they are variables that store memory addresses). 

    int *point;

To recall the memory address of a variable, we use the _ampersand operator_.

    int an_integer;
    int *point;

    an_integer = 10;
    point      = &an_integer;

To get the content of a memory address given by a pointer, we use an asterisk.

    *point;                                 // 10

The name of an array is the same as a pointer to that array. So `*array` is the same as `array[0]`, and `*(array+1)` is the same as `array[1]`, etc.

Pointers allows us to allocate memory for a program at runtime, rather than compile time - this is called _dynamic memory allocation_. `malloc` stands for _memory allocation_ and is a standard library function. It's prototype is

    void *malloc(size_t nbytes);

`malloc` returns a generic pointer, a pointer which points to any type of object. When `malloc` runs out of memory space, it returns a _null pointer_. Hence, we need to check for this condition at each `malloc` call.

`calloc` and `realloc` are other dynamic memory allocation functions.

It is best to free up the memory. To do so, use the `free` function, and then set the relevant pointer to `NULL`.

    #import <stdio.h>

    int main() {
        char *array_pointer;
        array_pointer = malloc(5 * sizeof(int));                // Array of 5 integers

        if( array_pointer == NULL ) {
            fprintf(stderr, "Out of memory\n");
            exit(8);
        }

        free(array_pointer);
        array_pointer = NULL;
    }

### Structures
Structures allow you to organise related objects. These can be variables or functions, etc. The definition of a structure should be at the top of the program.

    struct a_structure {                                        // Define structure
        char a_string[];
        int  an_integer;
        int  another_integer;
    }

    ...

    struct a_structure structure_name;                          // Declare a new variable

    structure_name.a_string        = "hello";                   // Assign values with dot-notation
    structure_name.an_integer      = 5;
    structure_name.another_integer = 10;

When passing a structure to a function, we usually pass a pointer to the structure, and use the arrow symbol `->` to access the contents of a pointer.

    char   str[];
    struct a_structure *point;                                  // Declare pointer of variable type `a_structure`

    point = &structure_name;                                    // Point to `structure_name`
    str   = point->a_string;

### Libraries
As well as the standard input/output library, we have others. Most often, you will want to use mathematical functions. These are included in the math library.

    #import <math.h>

To compile a program, we now need to use the `-lm` parameter so that the compiler knows to look for the `libm.*.so` library in the directory `/lib/`.

    gcc program.c -lm -0 program



## Resources

- [CY900 - Foundations of Scientific Computing](http://www2.warwick.ac.uk/fac/sci/csc/teaching/modules/cy900/current/)