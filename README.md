# C Programming Reference
By Marquez Jones

## Table of Contents
- [C Programming Reference](#c-programming-reference)
  * [Introduction](#introduction)
  * [Development Environment](#development-environment)
  * [More Robust Reference Material](#more-robust-reference-material)
  * [Specific References to Content Not Covered Here](#specific-references-to-content-not-covered-here)
  * [Program Structure (Hello World)](#program-structure--hello-world-)
  * [Data Types and Variables](#data-types-and-variables)
  * [Conditions and Loops](#conditions-and-loops)
  * [Functions](#functions)
  * [Header Files (Developing With Multiple Files)](#header-files--developing-with-multiple-files-)
  * [Pointers](#pointers)
  * [Arrays](#arrays)
  * [Strings](#strings) 
  * [Structs](#structs) \
<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


## Introduction
This is a quick and dirty introduction to programming in C. It assumes some familiarity with programming in general and seeks to expose a few intermediate topics as well as the basics. There is plenty of material available online already ,but this docuement is designed to be as brief as possible and easily referenceable for someone seeking to implement a basic program in C. This is a bare bones guide on developing in C.

## Development Environment
There are many ways to compile and run C such as using a full IDE or a text editor plus command line compiler(such as gcc). For the sake of ease of use and basic prototyping of the concepts below; I'll point to onlinegdb which is a web based development environment for multiple programming languages. 

NOTE: Ensure you set C as the used language 

Link: https://www.onlinegdb.com/

## More Robust Reference Material

Developer Insider has a nice C programming reference page that goes further into detail about C's features than what I will go into here.\
Website: https://developerinsider.co/c-and-cpp-insider/ 

The Developer Insider GitHub hosts many additional C example programs.  \
GitHub: https://github.com/developerinsider/C-Programming-Example

Geeksforgeeks also has a very indepth catalog of C concepts. \
Website: https://www.geeksforgeeks.org/c-programming-language/?ref=gcse

## Specific References to Content Not Covered Here
These are specific C features that I believe should be investigated even if I wasn't able to cover them in this guide as of now. \
Some of these may or may not be incorporated into this document at a later date. 

Arithmatic Operators: https://www.geeksforgeeks.org/operators-in-c-set-1-arithmetic-operators/ \
Logical Operators: https://www.geeksforgeeks.org/operators-in-c-set-2-relational-and-logical-operators/ \
Constants/Defines: https://www.geeksforgeeks.org/constants-in-c-cpp/ \
Bitwise Operators: https://www.geeksforgeeks.org/bitwise-operators-in-c-cpp/ \
Function Pointers: https://www.geeksforgeeks.org/function-pointer-in-c/ 

## Program Structure (Hello World)

In C, program execution begins execution at the start of the ```int main() ```; all C programs will have a main function that is implemented by the developers. In most cases, you can also pass in arguments to the main function in the format ,```int main(int argc, args[])```,but that won't be covered in this document. All C programs must have a main() function in order to be valid/begin as most compiled languages(C,C++,Rust,Go) need. Other languages like Python or Perl just being execution at the top of the file. 

Below is a couple hello world examples in two different contexts. I include both in the case you may be using C to program a microcontroller. 

### OS Context
If your running your application on a desktop thats running an OS like Windows, MacOS, or Linux, a simple program will eventually terminate at a 
``` return 0; ``` line. There are some exceptions, but this is for simple programs. You can also omit the return statement and the program will still terminate, but returning 0 is a better practice in the long run.

```
#include <stdio.h>
int main(){
  /* main application */
  printf("Hello World"); //note: ";" at the end of each executable line of code
  return 0; /* terminate program */
}
```
### Bare Metal/Embedded Context
For most basic embedded applications, your main loop will run forever instead of terminating. This allows the microcontroller to persistently execute your application for as long as it has power. There are more complex application involving Embedded OSes like an RTOS where a main thread can terminate, but that's not covered in this document. 

Think of the example where your microcontroller runs the brake system on your car; in this case, you would not want your microcontroller application to terminate causing your brakes to no longer function.
```
#include <stdio.h>
int main(){
  /* hardware initialiations */
  /* don't terminate until system loses power */
  while(1){
    /* application code */
    printf("Hello World");
  }
}
```

## Data Types and Variables
C allows you to store data, retrieve data, and do operations on data. This is necessary for basically all programs ever written. This data can be numbers, characters, or even addresses for other variables. 

One note about C in comparison to more "modern" programming and scripting languages is that C requires you to provide the data type to the variable. The data type tells the compiler what kind data is being stored and how operations should be performed such as addition or subtraction. Other languages might provide type inference like C++ with auto or with Rust; in those cases the compiler figures out what the data type is and holds it to the necessary rules. Languages interpretted like Python has dynamic data types where a variable change data types on the fly.

Declaring variables takes the form \
``` DATA_TYPE variable_name; ```

It's also generally recommended that you initialize the data as such \
``` int a = 0; ```

### Basic
C supports a large selection of data types as displayed in the code snippet below.

```
#include <stdio.h>
/* variables can be either local to a function or global */
/* global variables can be seen by the entire program while local are only seen by 
   their function/scope 
 */ 
int g = 5; 
int main(){
  /* all variables declared in main are considered local contrasted with global variables */
  /* integer values */
  int a = 1;
  int b = 2;
  int c = a + b;
  printf("c = %d\n",c);
   
  /* floats */ 
  float d_f = 3.7;
  printf("d = %0.2f\n",d_f);
  
  /* characters */
  /* note: arrays of characters terminated by 0 can make C strings */
  char m = 'm'; /* single quotes */
  char j = 'j'; 
  printf("%c%c\n",m,j);
  
  printf("global g = %d\n",g);
  return 0; /* terminate program */
}
```

### #include <stdbool.h>
The bool data type is not included by default in C. It must be included by adding ```#include <stdint.h> ``` at the top of your file. Booleans let you have a true/false variable in your code. It's not technically necessary since C interprets integer 0 as false and anything else as true, but booleans increase readability.

```
#include <stdio.h>
#include <stdbool.h>

bool global = false; //global variable 
int main(){
  bool local = true; //local variable to main
  printf("global is %d\n",global); 
  printf("local is %d\n",local);
  return 0;
}
```

### #include <stdint.h>
One of my favorite things to do in C is declare the size of my data types. By default, an int is a 16 bit integer value on most platforms but is not technically standardized. Including stdint.h allows you to declare signed and unsigned integers of varying sizes.

This is especially useful when reasoning about certain bitwise operations you'll generally see in embedded programs.

```
uint8_t a_u8 = 255; /* 8 bit unsigned number */
int16_t a_i16 = -1000; /* 16 bit signed number */
uint32_t a_u32 = 1000000; /* 32 bit unsigned number */
```

## Conditions and Loops 
Flow of control is a critical part of programming. Every program at some point will need a way to choose which block of code to execute based on some kind of condition; this is done via loops and dedicated conditional statements. 

NOTE: C has a dedicated boolean variable but also interpret numerical values that are not 0 as true and 0 as false

### Conditionals 

For conditional code execution, C provides 2 different statements and those are ``` if else ``` blocks and ``` switch ``` statements

#### if else statements
If statements can be a simple single block of execution or can be paired with ``` else ``` and ``` else if () ``` blocks.
```
#include <stdio.h>

int main(){
  int a = 3;
  int b = 3;
  
  /* single if */
  if ( a == b ){
    printf("a is equal to b\n"); 
  }
  
  
  /* if else */
  a = 5;
  b = 6;
  if ( a == b ){
    printf("a is equal to b\n"); 
  }
  else{
    printf("a is not equal to b\n");
  }
  
  /* if if else else */
  a = 7;
  b = 6;
  if ( a == b ){
    printf("a is equal to b\n"); 
  }
  else if(a == 7){
    printf("a is equal to 7\n");
  }  
  else{
    printf("a is not equal to b\n");
  }

}
```

#### switch statements 
Switch statements can be used to compare an input value against multiple other values and choose which block to execute based on a match.

```
#include <stdio.h>

int main(){
  char input = 'q';
  switch(input){
    case 'a':
      printf("a found \n");
      break; /* break necessary to stop switch from executing later lines of code */
    case 'b':
      printf("b found \n");
      break;
    case 'q':
      printf("q found \n");
      break;
    default: /* what to do if no match found */
      printf("no match found\n");
      break;
  }
}
```

### Loops 
Many times, you'll want to write a block of code that repeats an operation for some number of times or want to do a task until a condition is met. This could be iterating through an array or waiting for some kind of input. 

#### While Loop 
While loops execute until the input condition is met.

``` while(condition){ /*execute block of code */}```

```
#include <stdio.h>

int main(){
  int count = 0;
  
  /* while count is less than 10 , do operation */
  while(count < 10){
  
    printf("%d\n",count);
    count++;
    
  }
}
```
#### For Loop
For loops in C are actually very flexible in use but the most basic case would be iterating through some kind of range of numbers.

Typically take the form ``` for( starting index ; condition ; increment index ) { /* block of code */ } ```

```
int main(){
  
  for(int i=0; i < 10 ;i++){
    printf("%d\n",i);
  } 
}

```

## Functions 
Functions are extremely important part of writing clean, readable, and reusable code. They allow us to define a block of code that can be called later in our program multiple times. Functions can be passed arguments which are data from the rest of our program, and also return values. When using pointers/array pointers, they can also edit and "return" data as well. Pointers are explained after this section ,but it's worth mentioning them now.

Functions take the form 
```
RETURN_TYPE func_name(TYPE arg0, TYPE arg1, TYPE arg2, ....){
  return return_value;
}
```
Functions can also return nothing which we call void which is useful for executing blocks of code that might set up background systems or this might be used when the 
input is a pointer that stores our return.
```
void func_name(TYPE arg0,...){
  return;
}
```

When structuring code, functions need to be either included, defined, or prototyped before our main. 
NOTE: The compiler has to know that a function exists before it is called. 

### Function Prototype + Function Definition 
Prototyping a function is a nice way to tell the compiler that we have created a function without having to have the full definition before main. Some programmers prefer the main function to be the first true block of code in the file for organization purposes. 

```
#include<stdio.h>

/* add a and b together */
int foo(int a, int b);

/* print bar */
void bar(void); 

int main(){
    
    int a = foo(3,5); /* foo returns a value */
    printf("%d\n",a);
    
    bar(); /* bar return void/nothing */
  
}

int foo(int a, int b){
  return a + b;
}

void bar(void){
  printf("Bar\n");
}

```

### Function Definition Only
Another option is to just put the function defintions before the main function

```
#include<stdio.h>

/* add a and b together */
int foo(int a, int b){
  return a + b;
}

/* print bar */
void bar(void){
  printf("Bar\n");
}

int main(){
    
    int a = foo(3,5); /* foo returns a value */
    printf("%d\n",a);
    
    bar(); /* bar return void/nothing */
  
}

```

## Header Files (Developing With Multiple Files)
For better organization and reusability of code, it's often advised to separate the application into multiple files. This is also useful because you'll be able to start developing libraries that can be reused in multiple projects instead of being entangled into a single project. 

Most programming languages support importing other code. In C, this is accomplished by using header and source files. You prototype everything that will be visible in the ```.h``` file and actually implement it in a ```.c``` file. 

In the fib example, I only use functions; but you can also share variables, structs, and defines via header files. This file implements a fibonacci calculation. I did it iteratively for reasons. 

The process for compiling and running multiple c files is environment dependent. For onlinegdb and most IDEs, you can simply add extra files to the project. If you're using command line, gcc has more arguments. You could also create a makefile as well.

### fib.h 
```
/* include guards prevent the compiler from importing the file multiple times */
#ifndef FIB_H_ 
#define FIB_H_

/* here I provide a function prototype for fib() */
int fib(int n);

#endif /* FIB_H_ */
```

### fib.c
```
/* actual definition of our fibonacci function */
int fib(int n)
{
    int i = 3;
    int x = 1;
    int y = 1;
    while ( i <= n) {
        int temp = x+y;
        x = y;
        y = temp;
        i++;
    }
    
    return y;
}

```

### main.c

```
/* generally "" searches for file in same directory while <> is for files the compiler/IDE "knows" about */
#include<stdio.h>
#include "fib.h"

int main(){
  int result = fib(9);
  printf("fib result = %d",result);
}
```

## Pointers 
Pointers are a decently difficult part of C and a source of many errors, but are a necessary part of C due to how close the language is to the underying hardware. They are inherently used when using array data structures in C. They also allow pass by reference and provide an efficient way to pass large sums of data around programs. 

This will cover extremely basic parts of pointers. You can technically understand arrays and strings in C without going over pointers, but I believe one should understand this before going onto arrays and strings. 

### High Level 
A pointer variable in C is basically storing the ```memory address``` of a piece of data instead of referencing the data itself. Most functions you'll see will use pass by value where the developer provides a copy of the data to the function. With pointers, you'll providing the memory address where the data can be read and written to. 

Pointers are a memory address ,and memory addresses point to pieces of data. 

### Syntax 

To declare and dereference a pointer, we use the ``` * ``` operator. 

``` int *p; ``` declare p as a pointer to an int type 

If you have a normal variable, you can always access it's address with the ``` & ``` operator.

```
int *p;
int a = 5;
p = &a; /* sets pointer p to a's address so now p points to a */
```

### Basic Example 
NOTE: You can have a pointer for basially any data type in C. You can have pointers to int,float,char,etc. You can even have a pointer to a full struct of data, a pointer to a function, or a pointer to a pointer to a pointer to a pointer to a pointer if you wish.
```
#include <stdio.h>
/* basic case */
int main(){
  int a = 5; /* I have a variable a */
  int *p = &a; /* set  p to a's address */
  int b = *p; /* now we can dereference p and get the value stored at a*/
  
  /* these will be the same value */
  printf("*p = %d\n", *p); /* dereferenced p */
  printf("a = %d\n", a);
  printf("b = %d\n", b); 
}
```

### Function Example
You can also pass pointers into functions which gives the function the ability to edit the value directly or simply use it

```
#include <stdio.h>

/* foo will just use p */
int foo(int *p){
  if(p == NULL){ /* p pointed to an invalid address */
    return -1; // this may be a bad return code based on application 
  }
  return *p + 2; 
}

/* bar will edit the value of p */
void bar(int *p){
  if(p == NULL){ /* p pointed to an invalid address */
    return;
  }
  *p += 2; /* add to to value pointed to by p */
  return;
}

int main(){
  int a = 5; 
  int *p = &a;
  
  int b = foo(p);
  printf("a = %d\n", a);
  printf("b = %d\n", b);
  printf("*p = %d\n", *p);
  
  bar(p); /* will change value at p */
  printf("*p = %d\n", *p);

}

```

## Arrays
Arrays are the most common and basic data structure you'll see in most programming languages. They are useful for storing large amounts of the same type of data and doing operations on them. Imagine if you had a sensor and wanted to store multiple samples of that data and then further process the array of data later or just store it for another reason. This is why array data structures are necessary. 

As mentioned in the pointer section, arrays are actually implemented as pointers in C. So they point to blocks of multiple variables in memory. 

### Basics
In C, all indexes of the array must be the same kind of data. There are technically exceptions to that given union structs exist but you'd still have an array of unions. 

Arrays must also be of fixed size meaning the compiler must know the exact size of the array or it will not compile the program. There are more complex data structures that use dynamic memory allocation to make variable sized arrays but this document does not cover dynamic memory as of now. Some C applications also might not have access to dynamic memory in the case of a low powered microcontroller. 

Arrays take the form ``` TYPE name[SIZE]; ```  
You can also initialize the data as well ``` int numbers[4] = {0,1,2,3};  ```

### Example
```
#include <stdio.h>

/* it's advisable that you also pass in the array size into functions that use arrays */
int sum0(int data[], int size); 

/* you can also just pass it as a pointer variable as well */
int sum1(int *data, int size);

int main(){

  int numbers[5];
  
  /* C allows you to edit each location in the array if you didn't want to initialize it */
  for(int i=0; i < 5; i++){
     numbers[i] = i; /* populate the array */
  }
  
  int a = sum0(numbers,5);
  int b = sum1(numbers,5);
  
  printf("a = %d\n", a);
  printf("b = %d\n", b);

  return 0;
}

int sum0(int data[],int size){
  /* uses while loop to iterate */
  int i = 0; /* indexing starts a 0 */
  int res = 0;
  while(i < size ){
    res+= data[i];
    i++;
  }
  return res;
}

int sum1(int *data, int size){
  /* uses for loop to iterate */
  int res = 0;
  for(int i=0; i < size; i++){
    res += data[i];
  }
  return res;
}
```

## Strings
As far as C is concerend, strings are just arrays of characters terminated by a NULL character ('\0' or 0). There are also many many many library functions for just string manipulation that I will not go over in this document, but just know they exist in ``` string.h ```. 

If you "manually" created a string, it'd be this 

``` 
char str[] = {'I',' ','l','i','k','e',' ','C','\0'}; 
``` 

Which is a valid C string but C also has a shorthand for creating strings using ``` "" ``` semantics such as 

```
char str[] = "I like C";
```

### Example Program
```
#include <stdio.h> 

int main(){
  char *str = "By Marquez Jones"; /* you can also declare C strings with the pointer syntax */
  
  printf("%s\n",str); /* printf supports strings like we've been using all long */ 
  
  /* you can also manually parse for the end of a string */
  
  char *pstr = str; /* set pointer equal to pointer */
  
  while(*pstr){ /* while string not at null character */
    putchar(*pstr);
    pstr++;
  }

  return 0;
}

```

## Structs 
Fundamentally, C is a procedural language as opposed to an object oriented one, but does have a "class" style feature called structs for better organization of data. These allow you to bundle a set of logically connected variables into a single object for organization and passing around the program. Effectively structs are user defined data types that encapsulate multiple variables.

Practically any data type can be encapsulated into a struct including standard types, arrays, pointers, function pointers, and other structs. 
Structs can also be passed into functions like any other variable.

### Example struct
Let's create an example struct declared as such 
```
/* struct declaration */
struct example{
 char str[20]; /* include array of 20 characters */
 int data[5]; /* array of ints */ 
 int a;
 float b;
};
```

In your code, they would be instantiated as such if blank then you can modify member data with ```. ``` operator
```
struct example test; 
test.a = 5;
test.b = 3.7;
strcpy(test.str,"String"); /* strcpy function from string.h */ 

for(int i = 0;i < 5; i++){
 test.data[i] = i;
}
```

if you have a pointer to a struct ```->``` operator is used to access members instead 
```
struct example *p= &test;
p->a = 7;
```

You can also use the typedef keyword to declare a struct to avoid retyping "struct" when instantiating them

```
/* struct declaration */
typedef struct example{
 char str[20]; /* include array of 20 characters */
 int data[5]; /* array of ints */ 
 int a;
 float b;
}example_t; /* appending _t is just a common practice */
```

```
example_t test; 
```

### Example Program
```
#include <stdio.h> 
#include <string.h>

/* struct declaration done in global space or header file */
struct example{
 char str[20]; /* include array of 20 characters */
 int data[5]; /* array of ints */ 
 int a;
 float b;
};

int main(){
 struct example test;
 test.a = 5;
 test.b = 3.7;
 strcpy(test.str,"String"); /* strcpy function from string.h */ 

 for(int i = 0;i < 5; i++){
  test.data[i] = i;
 }
 
 printf("%s",test.str);
 
 return 0;
}

```
