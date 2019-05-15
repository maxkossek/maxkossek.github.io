---
layout: post
title: The C Programming Language (Kernighan & Ritchie)
date: 2019-04-30
categories: [book]
tags: [computer science]
author: "Max Kossek"
description: Book Notes for the book The C Programming Language by Kernighan & Ritchie
---

<div class="toc">
<strong>Table of Contents:</strong><br><br>
<ul>
<li><a href="#1---a-tutorial-introduction">1 - A Tutorial Introduction</a></li>
<li><a href="#2---types-operators-and-expressions">2 - Types, Operators and Expressions</a></li>
<li><a href="#3---control-flow">3 - Control Flow</a></li>
<li><a href="#4---functions-and-program-structure">4 - Functions and Program Structure</a></li>
<li><a href="#5---pointers-and-arrays">5 - Pointers and Arrays</a></li>
<li><a href="#6---structures">6 - Structures</a></li>
<li><a href="#7---input-and-output">7 - Input and Output</a></li>
<li><a href="#8---the-unix-system-interface">8 - The UNIX System Interface</a></li>
</ul>
</div>

## 1 - A Tutorial Introduction

### 1.1 Getting Started
C program consists of functions and variables.[^1] Function contains statements that specify operations, and variables store values. Program begins executing at main so every program must have a `main()` function.


### 1.2 Variables and Arithmetic Exceptions
`/* Comment */`
All variables must be declared before they are used. Common data types are int, float, char, short, long, double. While loops are notated by `while ( )`. Inside a printf statement a % indicates where an arguments is to be substituted `printf("%d %3.2f\n", fahr, celsius);` substitutes fahr into the %d location, and celsius is a float number with two numbers after decimal point, and a space location of at least 3 characters wide.


### 1.3 The for statement
For loop has similar notation `for (fahr=0; fahr <= 300; fahr = fahr + 20)`.


### 1.4 Symbolic Constants
A define line defines a symbolic name or symbolic constant for a string of characters `#define LOWER 0`. After this define line, every occurence of LOWER will be replaced with 0 in the program. Symbolic names are conventionally written in upper case letters to distinguish from variable names. There is no semi colon at the end of the define line.


### 1.5 Character Input and Output
Text stream is a sequence of characters divided into lines by new line characters. getchar reads the next input character from a text stream every time it is called `c = getchar();`. `putchar` prints a character each time called `putchar(c);`. `++variable` or `variable++` can be used to increment a variable. A for or while loop must have a body, so we use an indented semicolon as a null statement `\t;`. To check for equality == and != is used. These are different form = which is used for assignment statements. A Character written in single quotes represent an integer value -> 'A' has a ASCII value of 65. And is written as && and or is written as ||. Also conditional statements are written in form: 
```
if ( ); 
else if ( ); 
else;
```


### 1.6 Arrays
Declaring an array `int ndigit[10];`. This creates an array of 10 elements. Accessing these array values start at 0 and end at 9 for this example `ndigit[0]`. 


### 1.7 Functions
Properly designed functions remove to need to know how a job is done; knowing what is done can be sufficient for the output. Function definition has form:
```
return-type function-name(parameter declarations)
{
    declarations
    statements
}
```

Variables inside a function are local, reusing their name outside the scope doesn't cause a conflict. Usually every function should return a value, even the main function. The notation for returning is `return 0;`. Returning 0 usually implies a normal termination. Functions use a function prototype, which is written before the main function in the form `int power (int, int);`. The parameters don't have to be named, but they're data types have to be signified. 


### 1.8 Arguments - Call by Value
All functions arguments are passed by values, they are temporary variables rather than the originals. Parameters are local initialized variables in the called routine.


### 1.9 Character Arrays
A function that doesn't return a value is set to return type "void". The "\0" character marks the termination of a string. Must add this at the end of a char array if you want to be able to print out this array.


### 1.10 External Variables and Scope
Each local variable in a function only comes into existence once that function is called, and disappears when the function is exited. These variables are also called automatic variables. They do not retain their value with each function call, hence they have to set with each entry. External variables are declared outside the main function, and are globally accessible. They also exist permanently. The global variable's try has to be declared inside a function however `extern int max;`. The extern label isn't needed if the external variables are declared at the beginning of the source file.

Definition is where variable is created or assigned storage. Declaration is where variable is stated, but no storage allocated. 




## 2 - Types, Operators and Expressions

### 2.1 Variable Names
Variables names must start with letter and can include letters, digits and underscores. Use underscores to make long variable names more readable. Use short names for local variables (i.e. loop indices) and long names for external variables.


### 2.2 Data Types and Sizes
Basic data types in C:
- char - single byte holds character; 
- int - integer that reflects natural size of integers on host machine; 
- float - single precision floating point; 
- double - double precision floating point. 

Qualifiers 'signed' or 'unsigned' can be added to char or any integer. Unsigned numbers are always positive or zero and go to 2^n. Signed chars can be negative or positive and go from 2^(n-1) to 2^(n-1) - 1. Standard headers `<limits.h>` and `<float.h>` contain symbolic constants for these sizes.


### 2.3 Constants
A long constant has to be written with a terminal l or L `123556789L`. Unsigned constants end with terminal u or U and unsigned long end with ul or UL `1234u or 123456978ul`. Floating point constant contain decimal point (123.4), exponents (1e-2) and end with suffix f or F. Integer can be specified as octal or hexadecimal with a leading 0 (octal) or a leding 0x or 0X (hexadecimal). Character constants are written in single quotes and are evaluated based on ASCII value -> '0' is equal to 48 in ASCII. 

String constants are enclosed in double quotes `"Hello world."`. Constant expressions are evaluated at run time `#define SIZE 1000`. String constant have to end with a '\0'. `strlen(s)` in <string.h> returns the length of the string without taking into account '\0'. Enumeration constant is a list of constant integer values `enum boolean { NO, YES };`. The first value in enum has value 0, the next 1, the next 2 and so on. Values in enum can also be assigned names `enum months { JAN = 1; FEB, MAR, APR ... }`. 


### 2.4 Declarations
Variables and their types must be declared before use. Qualifier `const` can be added to signify that a value won't change `const double e 2.71828182845905;`. Const can also be used with array so that they don't change.


### 2.5 Arithmetic Operators
Standard binary arithmetic operators +, -, *, / and %. Integer division truncates fractions. Modulo operator % can't be applied to a float or double. *, / and % have higher precedence than + and -. Operators associate left to right.


### 2.6 Relational and Logical Operators
Standard relational operators >, >=, <, <= have higher precedence than equality operators == and !=. Relational operators have lower precendence than arithmetic operators. Logical operators && (and) and || (or) are evaluated left to right and evaluation stops as soon as result is known. For example `i < SIZE && (c=getchar()) != '\n'` checks if there is space in array first before getting another character. && has higher precendence than ||.


### 2.7 Type Conversions
Automatic conversions occur when narrower information is converted to wider information without losing information `float + int = int`. Expressions that lose information (long int to short) draw a warning but aren't illegal. Standard header `<ctype.h>` defines family of functions that make it easier to convert independent of character set. Float to int causes truncation of any fractional part. Type conversions can be forced with 'cast' `sqrt((double) n)` convert n from int to double before taking sqrt. 


### 2.8 Increment and Decrement Operators
Increment and decrement operators can be used as prefix and suffix `i++ ++i i-- --i`. i++ increments the value after it is used, while ++i increments the value before it is used. 


### 2.9 Bitwise Operators
Bitwise operators can only be applied to integral operands char, short, int and long. They are:
- & - bitwise AND;
- \| bitwise OR; 
- ^ bitwise XOR; 
- << left shift; 
- \>> right shift; 
- ~ one's complement 

Shift operators shift a bit by specified number of positions `x << 2` shift x value two positions to the left, which is equivalent to multiplying by 4. 


### 2.10 Assignment Operators and Expressions
Assignment operators can be used to compress assigning a new value to a variable `i = i + 2` is equal to `i += 2`. All binary operators can be used as assignment operators (*=, -=, %=, <<= etc.).


### 2.11 Conditional Expressions
Conditional expressions are used to shorten if else statements `expr1 ? expr2 : expr3` if expr1 is true, then expr2 is evaluated, else expr3 is evaluated. For example `z = (a > b) ? a : b;` sets z to the maximum of (a,b). 


### 2.12 Precedence and Order of Evaluation
--- Precedence Table ---
1. () [] -> .
2. ! ~ ++ -- + - * (type) sizeof
3. \* / %
4. \+ -
5. \>> <<
6. < <= > >=
7. == !=
8. &
9. \^
10. \|
11. &&
12. \|\|
13. ?:
14. = += -= *= /= %= &= ^= \|= <<= >>=
15. ,

Writing code that depends on order of evaluation is bad programming practice in any language. 




## 3 - Control Flow

### 3.1 Statements and Blocks
Semicolon is a statement terminator `x = 0;`. Braces { and } group declarations and statements together. 


### 3.2 If-Else
If-else is used to express decisions. To check if a expression is not equal to 0, we can use shortcut `if (expression)` in place of `(expression != 0)`. Else if associated with the closest if. The compiler doesn't take into account indentation, so use braces around nested if statements.


### 3.3 Else-If
Else-If is written as `else if` in C. If one of the else if statements evaluates to true, all other remaining expression are skipped. The else statement can be omitted. 


### 3.4 Switch 
Switch statement is a way to test if an expresion matches one of a number of branches
``` 
switch(expression) { 
    case const-expr: statements  
    default : statements 
}
```
A break statement causes an immediate exit from the switch `break;`. Unless break is invoked, all switch cases are checked, even if one case already evaluated to true. Best practices is to add a break; after the default statement, even if it is not necessary.


### 3.5 Loops - While and For
A `while` expression continues evaluation until it's condition is not met 
```
while (expression)
    statement
```
A `for` statement provides a starting condition, a testing condition, and a incrementing condition. Any of these conditions can be omitted, but the semicolons have to be inserted. Multiple expressions can be inserted inside a for expression by separating them with a comma.
```
for (initial_expr; test_expr; increment_expr)
    statement
```
While and for loops can be used interchangeably. For loops are preferred for situations where there is a simple initialization and incrementing step.


### 3.6 Loops - Do-While
In contrast to for and while loopse the `do-while` loop first executes the body and then tests a expression
```
do
statement
while (expression);
```
In practice the do-while loop is much less used than for and while loopse. 


### 3.7 Break and Continue
A break statement can be used to exit a for, while or do loop early `break;`. Break causes the innermost loop to be exited immediately. A continue statement is similar to break, it causes the next iteration of an enclosing loop to be executed `continue;`. Continue does not work on switch statements, and is used more rarely. It can be used to simplify complicated loops so that the program doesn't nest too deeply.


### 3.8 Goto and labels
A `goto` statement is never necessary and can be easily abused. It may have a use in moving out from a deeply nested structure, since a break statement only breaks out of the innermost loop. Goto statements make maintaining code more challenging and generally harder to understand.
```
for (...)
for (...) {
    if ()
        goto error;
}
error:
```


## 4 - Functions and Program Structure

Functions hide details of computations to break larger programs into smaller ones. 


### 4.1 Basics of Functions
Function definition has form:
```
return-type function-name (arguments)
{
    declarations and statements
}
```

If return type is omitted, int is assumed. The return expression `return expression;` converts the expression to the return type if necessary. If a functions fails to return a value, the value will likely be garbage. Compiling and load a C program stores in 3 different files can be done on UNIX with command.
```
$ cc main.c filename1.c filename2.c
```


### 4.2 Functions Returning Non-integers
To return a value of type other than int, we must declare the function to return that type. Additionally the calling routine must know that the function returns a non-int value. To explicitly state the return value: `double var_name, function_name(arguments);`.


### 4.3 External Variables
External variables are defined outside of a function. Functions are always external, C doesn't allow nesting of functions. External variables should be used when the variables are shared among many functions, and if a longer lifetime for the variables are needed. 


### 4.4 Scope Rules
The *scope* of a name is the part of the program within which a name can be used. Variables declared at the beginning of a function, and function parameters are local to their function. An external variable lasts from declaration until the the end of the file is compiled. To use an external variable from a different file or before it is declared the `extern` declaration has to be used. Declarations simply declare the type of a variable name or function for the rest of the file `extern double val[]`. Definitions declare the type and additionally create variable and set aside storage `double val[SIZE];`. There can only be one definition of an external variable among all the source files, but there can be more than one declaration of the variable. 


### 4.5 Header Files
Common definitions and declarations are placed in a *header* (file name extension .h). To include a header in a .c file we write `#include "header_name.h"`. For a moderate program size, one header is usually enough, for a much larger program more headers and organization are needed.


### 4.6 Static Variables
A `static` declaration of an external variable or function limits the scope of that object to the rest of the source file. Once declared as static no other routine will be able to access the static object. Static variables declared inside a function remain in existence rather than changing with each function call. 


### 4.7 Register Variables
A `register` declaration tells the compiler that the variable declared will be heavily used. This may result in faster and smaller programs. A variable is declared as register with `register int x;`. There are restrictions on the number and types of register variables depending on the machine.


### 4.8 Block Structure
Declarations of variables can follow the left brace of any compound statement, not just functions. For example:
```
if ( ) {
    int x;
    for (while x ...)
}
```
In the above example x is local to the if statement, and is unrelated to any other variable named x outside it's scope. Best practice is to avoid reusing names for local variables.


### 4.9 Initialization
When not explicitly initialized, external and static variables are initialized to 0. Automatic and register variables have undefined initial values. Variables can be defined and initialized at the same time; for example `int x = 1`. For external and static variables initialization is done once. For automatic and register variables, the value doesn't have to be a constant. It can be any other previously defined value and function. 

Array can be initialized with a list enclosed in braces, separated by commas: `int days[] = { 31, 28, 31, 5, 10, 1, 2 };`. If the size of the array is not explicitly stated, the compiler will count the number of variables in the initialization (7 in the above example). If there are fewer initialized variables than the size of the array, then the rest will be set to 0. There is no way to set a value in the middle of an array without initializing the preceding values. For char arrays, there is a shortcut for initialization by using a string `char pattern = "ould";`. 


### 4.10 Recursion
C functions can be used recursively. Each recursive call resets all the automatic variables. Recursion doesn't save storage, nor is is faster. It is however more compact and often easier to understand. 


### 4.11 The C Preprocessor
Most common features of preprocessor are: `#include`, which includes contents of a file; `#define` which replaces a token by a sequence of characters.

#### 4.11.1 File Inclusion
A file can be included by writing either `#include "filename"` or `#include <filename>`. Searching for the filename usually begins in the directory where the source program was found. 

#### 4.11.2 Macro Substitution
Definition has the form `#define name replacement text`. Scope of a defined name is until the end of the source file. A definition name doesn't have to be a constant in the strictest sense, for example `#define forever for (;;)` or `#define max(A, B) ((A) > (B) ? (A) : (B))`. The second example is also called a *macro*, where the replacement text changes based on the input values, similar to a function. 

#### 4.11.3 Conditional Inclusion 
Preprocessing itself can also be controlled with conditional statements. If a `#if` statement is non-zero, then subsequent lines until an `#endif` or `elif` are included. This is useful to avoid including files multiple times:
```
#if !defined(HDR)    /* Can also be written #ifndef HDR */
#define HDR
/* hdr.h content */
#endif
```



## 5 - Pointers and Arrays

A *pointer* is a variable that contains the address of a variable. 


### 5.1 Pointers and Addresses
Typical machine has array of consecutive numbered or addressed memory cells that can be manipulated individually or as a group. One byte is a char, two bytes can be a short and four adjacent bytes can be a long. The unary operator & gives the address to an object, `p = &c;` assigns the address of c to variable p; p now points to c. Unary operator *, is the indirection or dereferencing operator. When applied to a pointer, * accesses the object the pointer points to.
```
int x = 1, y = 2, z[10];
int *ip;        /* ip is pointer to int */
ip = &x;        /* ip points to x */
y = *ip;        /* y is = 1 */
*ip = 0;        /* x is = 0 */
ip = &z[0];        /* ip points to z[0] */
```

Every pointer points to a specific data type (expection: pointer to void can hold any type, but can't be dereferenced). If *ip points to x, then it can be placed anywhere x could `*ip = *ip + 10;`, `y = *ip + 1`, `(*ip)++`. Can also make another pointer with the same pointer location `iq = ip`.


### 5.2 Pointers and Function Arguments
C passes arguments to functions by value, hence a function can not alter a variable when called. To alter the values, we have to pass pointers to the values that we want to change in memory `swap(&a, &b);`. 
```
void swap(int *px, int *py)
{
    int temp;
    temp = *px;
    *px = *py;
    *py = temp;
}
```


### 5.3 Pointers and Arrays 
Any operation that can be achieved by array subscripting can also be done with pointers (using pointers is faster, but harder to understand). 
```
int a[10];    /* Define array of size 10 */
int *pa;    /* pa is pointer to int */
pa = &a[0];    /* pa points to zero element of a */
/* pa = a; this is equivalent to the above line */
x = *pa;    /* copies a[0] into x */

a[i];        /* points to a[i], equivalent to *(a+i) */
&a[i];        /* points to a+i, equivalent to a+i */
pa[i];        /* points to a[i], equivalent to *(pa+1) */
```

There is a difference between array names and pointers: pointer is a variable so `pa = a` and `pa++` are legal. These constructions are illegal for array names because they aren't variables. For function parameters, `char s[];` and `char *s;` are equivalent. Similarly both `f(&a[2])` and `f(a+2)` both are equivalent, passing the subarray starting at a[2] to the function. It is possible to index backwards an array, for example `p[-1];` would get the last element in the array. It is illegal to refers to objects out of bounds.


### 5.4 Address Arithmetic
If p pointer to array, then `p++` makes p point to next element and `p+=i` makes p point to the element i elements passed the current one. To initialize a character pointer we can write `static char *pointer_name = char_array_name;`, or equivalently `static char *pointer_name = &char_array_name[0];`. 

Pointers and integers are not interchangeable. Zero is the only exception, because we can compare and assign a pointer to 0. A common standard is to use `NULL` defined in `stdio.h>` instead of 0 for a pointer. Any pointer can be compared for equality or inequality with zero (==, !=, <=, > etc.). Pointer manipulations automatically take into account the type of object they are pointing to (int, char, double are all valid). 

Legal Pointer Operations:
1. Assignment of pointers of same type
2. Adding / Subtracting pointer with integer
3. Subtracting or comparing two pointers to members of same array
4. Assigning or comparing to zero (or NULL).


### 5.5 Character Pointers and Functions
String constant such as "Hello World" is terminated with a `\0` in an array. There is a difference when defining a string with a pointer or an array. An array definition `char hello[] = "hello world";` creates a sequence of char's and `\0`. Individual characters can be changed, but it always refers to same storage. A pointer assignment `char *hello = "hello world";` initializes to point to the first char in the string. It can be modified to point to somewhere else in the string, but it can't modify the string contents. Incrementing and decrementing is legal as usual `*p++` or `*--p`.


### 5.6 Pointer Arrays; Pointers to Pointers
Pointers are variables, so they can be stored in arrays. In an array of pointers, elements can be modified by exchanging the pointers. 


### 5.7 Multi-dimensional Arrays
C has rectangular multi-dimensional arrays (less used in practice). In multi-dimensional arrays, each elements stores another array. The subscripts are `array[i][j]`, where i states the row and j the column. To pass an array to a function, the number of columns must be included; the number of rows is irrelevant. Hence `function(int array[i][j])`, `function(int array[][j])` and `function(int (*array)[j])` are equivalent. The parenthesis are necessary because brackets `[]` have higher precedence than *.


### 5.8 Initialization of Pointer Arrays
To initialize a pointer array:
```
static char *name[] = {
    "Illegal month",
    "January", "February", "March",
    "April", "May", "June"
};
```


### 5.9 Pointers vs. Multi-dimensional Arrays
There's differences between two-dimensional arrays `int a[i][j]` and an array of pointers `int *b[i]`. Both `a[i][j]` and `b[i][j]` are legal references to an int. But `a` is truly two-dimensional: i*j int locations are set aside, and 20*i+j is used to find element `a[i,j]`. The pointer definition `b` only sets aside 10 uninitizalied pointers; each individual row of the pointer array can have varying lengths (i.e could have 0 elements or 50). This is an advantage, if fixed sizes of the array are unnecessary.


### 5.10 Command-line Arguments
`main` is called with two arguments: 1. `argc` or argument count, is the number of command line arguments; 2. `argv` or argument vector, is a pointer to an array of char strings, each containing an argument. For example `echo hello, world` prints `hello, world`. `argc` is 3, `argv[0] = "echo"`, `argv[1] = "hello, "` and `argv[2] = "world"`. 


### 5.11 Pointers to Functions
A function is not a variable, but a pointer can be assigned, passed and returned by functions. For example we can pass a pointer to a function by `void function(arguments, void (*function2)(arguments))`.


### 5.12 Complicated Declarations
Because of precedence, declarations can be hard to read. `int *f();` is a function returning a pointer to an int; `int (*pf)();` is a pointer to a function returning an int. `dcl` converts a C declaration into a word description.




## 6 - Structures
*Structure* is collection of one or more variables grouped together under a single name. 


### 6.1 Basics of Structures
To create an object for a point:
```
struct point {
    int x;
    int y;
};
    struct rect {
    struct point pt1;
    struct point pt2;
};
struct rect screen;
screen.pt1.x;
```

Keyword `struct` intoduces structure declaration. Structure tag `point` names the structure (optional). Variables inside the structure are called members. To define a variable with a strcture type `struct point pt;` or for unnamed structures `struct maxpt = { 320, 200 };`. To refer to an member of a strcture use syntax `structure-name.member`. For example to print the coordinates `printf("%d,%d", pt.x, pt.y);`. Structures can also be nested inside other structures.


### 6.2 Structures and Functions
Structures have to be copied or assigned as a unit. They can not be used in comparison operations. Structures can be passed and returned by functions.
```
struct addpoint(struct point p1, struct point p2)
{
    p1.x += p2.x;
    p1.y += p2.y;
    return p1;
}
```

Structure pointers are like pointers to ordinary variables `struct point *pp;`. If pp points to point structure, then `*pp` is structure, `(*pp).x` and `(*pp).y)` are the members. We can also use `p->member-of-structure` as a convenient shorthand. For example `*pp->x` is equal to `(*pp).x`. Operators `.` and `->` are at the top of precedence hierarchy. Hence `++p->len` increments len, not p. Use `(++p)->len` to increment p.


### 6.3 Arrays of Structures
Structures can also be used in arrays:
```
struct key {
    char *word;
    int count;
} keytab[] = { "auto", 0 }, { "break", 0 }, { "case", 0 }, { "char", 0 };
```

C provides compile time unary operator `sizeof` to compute the size of any object. 


### 6.4 Pointers to Structures
When a program returns a complicated type, we can use alternate style:
```
struct key *
binsearch(char *word, struct key *tab, int n)
```
to make the program easier to read.


### 6.5 Self-referential Structures
To keep a list sorted, we can place elements into their proper position in the order as they arrive. A *binary tree* contains one node per element; with each node containing:
- pointer to data of the element
- pointer to left child node
- pointer to right child node

Binary search trees can significantly reduce the time of search O(logn). However if the tree becomes "unbalanced", the running time will grow closer to that of a linear search O(n). A node is most conveniently written as a structure:
```
struct node {
    char *word;
    struct node *left;
    struct node *right;
};
```

### 6.6 Table Lookup
A hash-search converts an incoming name into a small non-negative integer, used to index into an array. Standard way to walk along a linked list is a for loop `for (ptr = head; ptr != NULL; ptr = ptr->next)`.


### 6.7 Typedef
`typedef` is a facility that creates new data type names; `typedef int Length;` makes the name Length a synonym for int. `typedef char *String;` makes String a synonym for a character pointer. Typedef does not create a new type; it only adds a name for an existing type similar to `#define`. 


### 6.8 Unions 
*Union* is a variable that holds objects of different types and sizes; the compiler keeps track of size and alignment.
```
union u_tag {
int ival;
float fval;
char *sval;
} u;
```

In the above example, the compiler will make u big enough to hold the largest data type. Members of a union are accessed by `union-name.member` or `union-pointer->member`.


### 6.9 Bit-fields
*Bit-field* is a set of adjacent bits within a single storage unit called a "word". Field are referenced like structures `flags.is_keyword`. Bit-fields can sometimes be used to effectively store information on an identified.
```
struct {
unsigned int is_keyword : 1;
unsigned int is_extern : 1;
unsigned int is_static : 1;
} flags;
```



## 7 - Input and Output


### 7.1 Standard Input and Output
Simplest input mechanisms is reading one character at a time with `int getchar(void)`. Getchar returns the value of the char, or else EOF when it reaches the end of the file. To get input from a file instead of the keyboard use `prog <infile`. To output a single character use function `int putchar(int)`. Putchar returns the character written, or EOF if an error occurs. To output characters into a file instead of the default screen write `prog >outfile`. `putchar` and `printf` can be interleaved. To use the input/output functions the line `#include <stdio.h>` is required. 


### 7.2 Formatted Output - printf
`printf` converts, formats and prints it's arguments into the standard output, and returns the number of characters printed. `sprintf` functions similarly to `printf` but stores the output in a string. The string inside printf contains either ordinary characters, or a conversion specification (begins with a `%`). Parameters for the conversion specification in order:
1. Minus sign: specifies left adjustment
2. Number: specifies minimum field width
3. Period: separates field with from precision
4. Number/Precision: number of characters from a string, or the number of digits after the decimal point
5. Letter: `h` to print as short, `l` to print as a long


### 7.3 Variable-length Argument Lists
Gives an example of writing a minimal version of printf, that uses the conversion specifications as arguments to print output.


### 7.4 Formatted Input - Scanf
Function `scanf` is the input analog of `printf`. `scanf` reads characters from the standard input, and stores the results. It stops when it exhausts it's format string, or an input fails to match a control specification. Similar to printf, scanf has conversion character rules. Scanf returns the number of successfully matched and assigned input items. Arguments to scanf must be pointers.
```
/* Read in dates of form: 25 Dec 1988 */
int day, year;
char monthname[20];

scanf("%d %s %d", &day, monthname, &year);
```


### 7.5 File Access
To access a file outside the standard input and output we can use other functions. `fopen` takes an external name such as `filename.c` and returns a pointer to read & write the file. To declare a file pointer:
```
#include <stdio.h>
FILE *fp;
FILE *fopen(char *name, char *mode);

fp = fopen(filename, mode);
```

The available modes of fopen include "r" read, "w" write and "a" append. `getc` returns the next character from a specified file `int getc(FILE *fp)`. `putc` writes a character to a specified file `int putc(int c, FILE *fp)`. `fclose` breaks the connection between the file pointer and the external name making space for another file to be read `int fclose(FILE *fp)`. The functions `getchar` and `putchar` already encountered can be defined in terms of getc, putc, stdin and stdout:
```
#define getchar() getc(stdin)
#define putchar(c) putc((c), stdout)
```


### 7.6 Error Handling - Stderr and Exit
`stderr` writes output on the screen even if the standard output is redirected. Standard library function `exit` terminates a program when it is called. It returns a value of 0 if no errors occured; else it returns a non-zero value for abnormal situations. Inside the `main` function, return and exit are equivalent. However exit can also be called from within other functions. 


### 7.7 Line Input and Output
Standard library includes input and output function `fgets` and `fputs` that are similar to `getline`. `char *fgets(char *line, int maxline, FILE *fp)` reads at most `maxline` characters` starting at the next input line from file `fp` into char array `line`. `int fputs(char *line, FILE *fp)` writes the string `line` to output file `fp`. 


### 7.8 Miscellaneous Functions

#### 7.8.1 String Operations
Functions in `<string.h>`:    
`strcat(s,t)`: concatenate t to end of s   
`strncmp(s,t)`: return negative, zero, positive for s<t, s==t, s>t   
`strcpy(s,t)`: copy t to s   
`strlen(s)`: return length of s    
`srtchr(s,c)`: return pointer to first c in s, or NULL    

#### 7.8.2 Character Class Testing and Conversion
Function in `<ctype.h>`:    
`isalpha(c)` : non-zero if c is alphabetic   
`isdigit(c)` : non-zero if c is digit   

#### 7.8.5 Storage Management
`malloc` and `calloc` obtain blocks of memory dynamically. `void *malloc(size_t n)` returns a pointer to n bytes of uninitialized storage, or NULL. `void *calloc(size_t n, size_t size)` returns a pointer to enough free space to hold the array of n objects, or NULL. Pointers for malloc or calloc have the correct size based on data type, but have to be cast to the right type:
```
int *ip;
ip = (int *) calloc(n, sizeof(int));
```

`free(p)` frees the space pointer to by p. Best practice is to only free space that was obtained by calls to malloc or calloc.

#### 7.8.6 Mathematical Functions
Functions in `<math.h>`:   
`sin(x)` & `cos(x)` : sine and cosine of x in radians   
`exp(x)` & `log(x)` : exponential function e<sup>x</sup> and natural logarithm   
`power(x,y)` & `sqrt(x)` : x<sup>y</sup> and x<sup>1/2</sup>    



## 8 - The UNIX System Interface


### 8.1 File Descriptors
In UNIX operating systems, all input and output is read and written by files. To write to a file, the system checks permission and gives a small positive integer called a file descriptor (similar to file pointer). 


### 8.2 Low Level I/O - Read and Write
Input and output use C functions `read` and `write`. The return value for both is the number of bytes transferred. 
```
int n_read = read(int fd, char *buf, int n);
int n_written = write(int fd, char *buf, int n);
``` 


### 8.3 Open, Creat, Close, Unlink
Files different from standard input, standard output and error must be explicitly opened. Use system calls `open` and `creat`. Usually there is a limit on the number of concurrent files that can be opened. Function `close(int fd)` breaks the connection between file descriptor and the program.


### 8.4 Random Access - Lseek
`lseek` allows a file to be read and written in any order `long lseek(int fd, long offset, int origin);`. It returns a long that gives the new position in the file, or -1 if an error occurs.


### 8.5 Example - An implementation of Fopen and Getc
Functions and variables in `<stdio.h>` preferablly start with a underscore to avoid colliding with the user's program. 


### 8.6 Example - Listing Directories
UNIX command `ls` prints the names of files in a directory. A directory is a file that contains a list of filenames, and where they are located. The location of a file is the *inode list*, which stores all the information about the file.


### 8.7 Example - A Storage Allocator
Space that `malloc` manages may no be adjacent. Each block of storage contains a pointer to the next block. The last storage block points to the first block. When the user request space, the adjacent blocks are scanned until a big enough block is found. If the block is too big, it is split and returned to the user, else it is simply returned.


[^1]: Kernighan, B. W., & Ritchie, D. M. (2006). The C programming language.









































