# C Programming Reference

## Introduction
This is a quick and dirty introduction to programming in C. It assumes some familiarity with programming in general and seeks to expose a few intermediate topics as well as the basics. There is plenty of material available online already ,but this docuement is designed to be as brief as possible and easily referenceable for someone seeking to implement a basic program in C.

## Development Environment
There are many ways to compile and run C such as using a full IDE or a text editor plus command line compiler(such as gcc). For the sake of ease of use and basic prototyping of the concepts below; I'll point to onlinegdb which is a web based development environment for multiple programming languages. 

NOTE: Ensure you set C as the used language 

Link: https://www.onlinegdb.com/

## More Robust Reference Material
Website: https://developerinsider.co/c-and-cpp-insider/ \
GitHub: https://github.com/developerinsider/C-Programming-Example

## Program Structure (Hello World)

In C, program execution begins execution at the start of the ```int main() ```; all C programs will have a main function that is implemented by the developers. In most cases, you can also pass in arguments to the main function in the format ,```int main(int argc, args[])```,but that won't be covered in this document. All C programs must have a main() function in order to be valid/begin as most compiled languages(C,C++,Rust,Go) need. Other languages like Python or Perl just being execution at the top of the file. 

### OS Context
Below is a couple hello world examples in two different contexts. I include both in the case you may be using C to program a microcontroller. 
If your running your application on a desktop thats running an OS like Windows, MacOS, or Linux, a simple program will eventually terminate at a 
``` return 0; ``` line. There are some exceptions, but this is for simple programs.

```
int main(){
  /* main application */
  printf("Hello World");
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

## Data Types

```
int main(){
  /* main application */
  printf("Hello World");
  return 0; /* terminate program */
}
```

## Functions 

## Conditional and Loops 

## Header Files (Developing With Multiple Files)

## Structs

## Function Pointers

