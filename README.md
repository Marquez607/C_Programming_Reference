# C Programming Reference

## Introduction
This is a quick and dirty introduction to programming in C. It assumes some familiarity with programming in general and seeks to expose a few intermediate topics as well as the basics. There is plenty of material available online already ,but this docuement is designed to be as brief as possible and easily referenceable for someone seeking to implement a basic program in C. This is a bare bones guide on developing in C.

## Development Environment
There are many ways to compile and run C such as using a full IDE or a text editor plus command line compiler(such as gcc). For the sake of ease of use and basic prototyping of the concepts below; I'll point to onlinegdb which is a web based development environment for multiple programming languages. 

NOTE: Ensure you set C as the used language 

Link: https://www.onlinegdb.com/

## More Robust Reference Material

Developer Insider has a nice C programming reference page that goes further into detail about C's features than what I will go into here.\
Website: https://developerinsider.co/c-and-cpp-insider/ 

Their GitHub hosts many additional C example programs.  \
GitHub: https://github.com/developerinsider/C-Programming-Example

Geeksforgeeks also has a very indepth catalog of C concepts. \
Website: https://www.geeksforgeeks.org/c-programming-language/?ref=gcse

## Program Structure (Hello World)

In C, program execution begins execution at the start of the ```int main() ```; all C programs will have a main function that is implemented by the developers. In most cases, you can also pass in arguments to the main function in the format ,```int main(int argc, args[])```,but that won't be covered in this document. All C programs must have a main() function in order to be valid/begin as most compiled languages(C,C++,Rust,Go) need. Other languages like Python or Perl just being execution at the top of the file. 

Below is a couple hello world examples in two different contexts. I include both in the case you may be using C to program a microcontroller. 

### OS Context
If your running your application on a desktop thats running an OS like Windows, MacOS, or Linux, a simple program will eventually terminate at a 
``` return 0; ``` line. There are some exceptions, but this is for simple programs.

```
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
C allows you to store data, retrieve data, and do operations on data. This is necessary for basically all fundamental programs ever written. This data can be numbers, characters, or even addresse for other variables. 

One note about C in comparison to more "modern" programming and scripting languages is that C requires you to provide the data type to the variable. The data type tells the compiler what kind data is being stored and how operations should be performed such as addition or subtraction. 

Declaring variables takes the form \
``` DATA_TYPE variable_name; ```

It's also generally recommended that you initialize the data as such \
``` int a = 0; ```

### Basic
C supports a large selection of data types as displayed in the code snippet below.

```
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
  printf("%c%c\n,m,j);
  
  printf("global g = %d\n",g);
  return 0; /* terminate program */
}
```

### #include <stdbool.h>
The bool data type is not included by default in C. It must be included by adding ```#include <stdint.h> ``` at the top of your file. Booleans let you have a true/false variable in your code. It's not technically necessary since C interprets integer 0 as false and anything else as true, but booleans increase readability.

```
#include <stdbool.h>

bool global = false; //global variable 
int main(){
  bool local = true; //local variable to main
  printf("global is %d\n",gloabl); 
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
Flow of control is a critical part of programming. Every program at some point will need a way to choose which block of code to execute based on some kind of condition

### Conditionals 

### Loops 

## Functions 

## Header Files (Developing With Multiple Files)

## Pointers 

## Strings

## Geeksforgeeks References to More Intermediate/Advanced Content
These are specific C features that I believe should be investigated even if I wasn't able to cover them in this guide as of now. \
Constants/Defines: https://www.geeksforgeeks.org/constants-in-c-cpp/ \
Structs: https://www.geeksforgeeks.org/structures-c/ \
Bitwise Operators: https://www.geeksforgeeks.org/bitwise-operators-in-c-cpp/ \
Function Pointers: https://www.geeksforgeeks.org/function-pointer-in-c/ 
