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

    ```c
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
    ```

### Strings
There are several string specific functions which are good to know:

    strcopy(new_str,str);           // Copies the string `str` to `new_str`
    strcat(new_str,str);            // Adds the string `str to `new_str`
    strlen(str);                    // Returns the length of the string `str`

### Operators
The usual mathematical operators are present, listed below:

    
 
## Resources

- [CY900 - Foundations of Scientific Computing](http://www2.warwick.ac.uk/fac/sci/csc/teaching/modules/cy900/current/)