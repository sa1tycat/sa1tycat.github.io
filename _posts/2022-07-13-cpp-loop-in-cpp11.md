---
title: C++之循环新特性(C++11)
author: saltycat
date: 2022-07-13 20:47:59 +0800
categories: [C++]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io
---



# C++11 range-based for loops

## Syntax

[1]: https://en.cppreference.com/w/cpp/language/range-for

To simplify, the basic syntax for range-based loop is `for(range-declaration : range-expression)`, where:

range-declaration : a [declaration](https://en.cppreference.com/w/cpp/language/declarations) of a named variable, whose type is the type of the element of the sequence represented by *range-expression*, or a reference to that type. Often uses the [auto specifier](https://en.cppreference.com/w/cpp/language/auto) for automatic type deduction,

range-expression : any [expression](https://en.cppreference.com/w/cpp/language/expressions) that represents a suitable sequence (either an array or an object for which `begin` and `end` member functions or free functions are defined, see below) or a [braced-init-list](https://en.cppreference.com/w/cpp/language/list_initialization).

For example, 

[2]: https://www.cprogramming.com/c++11/c++11-ranged-for-loop.html



```c++
vector<int> v = {1, 2};
for(int i : vec) {
    cout << i;
}
```

In this loop, variable i take on the value of each element of v from the beginning to the end.

If you are not sure which type you should use for range-declaration, you can simply use `auto`, like:

```c++
string s = "abc";
for(auto i : s) {
    cout << i;
}
```

Worth noticing that the variable for range-declaration only receive the value of the element of the container, thus you **cannot modify** the elements of the container in the loop, unless you pass the reference like this:

```c++
vector<int> vec;
vec.push_back( 1 );
vec.push_back( 2 );
 
for (int& i : vec ) 
{
    i++; // increments the value in the vector
}
for (int i : vec )
{
    // show that the values are updated
    cout << i << endl;
}
```

In this case, the elements in vec are modified into `{2, 3}` respectively.

## for-each Loops

The `for-each` loop is especially useful for iterating through list-like structures such as arrays or `std::vector`. Although regular `for` loops are perfectly capable of this task, `for-each` loops provides a more convenient syntax:

```c++
int fibonacci[5] = {0, 1, 1, 2, 3};
for (int number : fibonacci) {
  std::cout << number;
}
```

The output of the above example is:

```
01123
```

This `for-each` loop iterates through an array called `fibonacci` using the counter variable `number`.

## The auto Keyword

When iterating through an array using the `for-each` loop, the counter variable may be declared using the `auto` keyword.

`auto` lets C++ deduce the type of the variable based on the type of the list it belongs to.

Here is the previous example written using `auto`:

```C++
int fibonacci[5] = {0, 1, 1, 2, 3};
for (auto number : fibonacci) {
  std::cout << number;
}
```
