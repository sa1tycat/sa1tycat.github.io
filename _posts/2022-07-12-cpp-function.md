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

definitions -> **.cpp** file (with the same name as the header file where they are declared)

```c
#include <iostream>
#include "functions.hpp" // don't forget!
 
int main() {
 
  std::cout << say_hi("Sabaa");
 
}
```

{: file='main.cpp'}



```c++

std::string say_hi(std::string name); // function declaration
 
```

{: file='functions.hpp'}



```C++
#include <string>
#include "functions.hpp" 
 
std::string say_hi(std::string name) { // function defintion
  return "Hey there, " + name + "!\n";
}
```

{: file='functions.cpp'}

when compiling this 3 files in a bash, you need to tell g++ all of the files you need to compile by `g++ main.cpp function.cpp` or `g++ main.cpp function.cpp -o test`, then run the executable by `./a.out` or `./test`.

## Inline Functions

 *inline function* （内联函数）is a function definition, usually in a header file, qualified by `inline` like this:

```c++
inline 
void eat() {
 
  std::cout << "nom nom\n";
 
}
```

Using `inline` advises the compiler to insert the function’s body where the function call is, which sometimes helps with execution speed (and sometimes hinders execution speed). Therefore, always use it after you have checked that it DOES IMPROVE the speed).

inline functions are usually member functions, functions inside of classes. For example:

```c++
void Cookie::eat() {std::cout << "nom nom\n";} // eat() belongs to the Cookie class:
```
{: file='cookie_functions.hpp'}

member functions are automatically inlined.

ALWAYS add inline keyword when inlining functions that are not member functions in header files.

## Default Arguments

Default arguments are values assigned to parameters when the function is declared and defined:

```c++
// Declaration
void intro(std::string name, std::string lang = "C++");
 
// Definition
void intro(std::string name, std::string lang) {
  std::cout << "Hi, my name is "
            << name
            << " and I'm learning "
            << lang
            << ".\n";
}
```

In example above, if we call the functions by `intro("Bob")`(leaving the argument of lang blank), the function will run with the default value (`lang = "C++"`). However, it'll also work if you add the argument for lang. In that case, the function will replace "C++" with the argument you added.

ALWAYS put the default arguments in the last place.

## Function Overloading

In a process known as *function overloading*（函数重载）, you can give multiple C++ functions the same name. Just make sure at least one of these conditions is true:

- Each has different type parameters.
- Each has a different number of parameters.

Overloading enables you to change the way a function behaves depending on what is passed in as an argument:

```c++
void print_cat_ears(char let) {
  std::cout << " " << let << "   " << let << " " << "\n";
  std::cout << let << let << let << " " << let << let << let << "\n";
}
 
void print_cat_ears(int num) {
  std::cout << " " << num << "   " << num << " " << "\n";
  std::cout << num << num << num << " " << num << num << num << "\n";
}
```

## Function Templates

Though overloading enables us to implement one functionality through the same name of functions that hold different types of parameters, it can be quite annoying to repeat the nearly same codes over and over again. Thanks to the templates, we can now use one functionality through one function name with different types of arguments.

When building a template, unlike other functions, a template is **completely declared and defined in a header file**. And it looks like this:

```c++
template <typename T>
return_type some_function_name(T item1, T item2) {
  // do stuff with item1 and item2
}
```

{: file='functions.hpp'}

For example we want to create a function that returns the maxium between two elements, we can build a template like this:

```c++
template <typename T>
T max(T a, T b) {
    return a>b ? a:b;
}
```

As long as the type can be used with the methods expected, we can use the function with any type we want.

