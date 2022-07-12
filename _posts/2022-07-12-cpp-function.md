---
title: C++之函数
author: saltycat
date: 2022-07-12 19:20:37 +0800
categories: [C++]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io
---

## Function Declaration & Definition

A C++ function has two parts:

- Function declaration（函数声明）
- Function definition（函数定义）

The declaration includes the function’s name, return type, and any parameters（参数）.

The definition is the actual body of the function which executes when a function is called. The body of a function is typically enclosed in curly braces.

```c++
#include <iostream>
 
// function declaration
void blah(); 
 
// main function
int main() {
  blah();
}
 
// function definition
void blah() {
  std::cout << "Blah blah";
}
```



## Function Arguments

In C++, the values passed to a function are known as arguments（实际参数）. They represent the actual input values.

## Function Declarations in Header file

C++ functions typically have two parts: declaration and definition.

declarations -> header files (**.hpp** or **.h**) 

definitions -> **.cpp** file (with the same name of the header file where they are declared)

```C++
// ~~~~~~ main.cpp ~~~~~~
 
#include <iostream>
#include "functions.hpp"
 
int main() {
 
  std::cout << say_hi("Sabaa");
 
}
 
 
// ~~~~~~ functions.hpp ~~~~~~
 
// function declaration
std::string say_hi(std::string name);
 
 
// ~~~~~~ functions.cpp ~~~~~~
 
#include <string>
#include "functions.hpp"
 
// function defintion
std::string say_hi(std::string name) {
 
  return "Hey there, " + name + "!\n";
 
}
```

when compiling this 3 files in a bash, you need to tell g++ all of the files you need to compile by `g++ main.cpp functions.hpp function.cpp` or `g++ main.cpp functions.hpp function.cpp -o test`, then run the executable by `./a.out` or `./test`.

