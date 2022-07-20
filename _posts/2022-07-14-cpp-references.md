---
title: C++之引用
author: saltycat
date: 2022-07-14 19:17:51 +0800
categories: [C++]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io
---



## References

A *reference*（引用） variable is an alias（别名） for something else, that is, another name for an already existing variable.

Suppose we have an `int` variable already called `songqiao`, we can create an alias to it by using the `&` sign in the declaration:

```c++
int &sonny = songqiao;
```

So here, we made `sonny` a reference to `songqiao`.

Now when we make changes to `sonny` (add 1, subtract 2, etc), `songqiao` also changes.

Two things to note about references:

1. Anything we do to the reference also happens to the original.
2. Aliases cannot be changed to alias something else.

## Pass-By-Reference

Previously, when we passed parameters to a function, we used normal variables and that’s known as *pass-by-value*. But because the variables passed into the function are out of scope, we can’t actually modify the value of the arguments.

*Pass-by-reference*（引用传参） refers to passing parameters to a function by using references. When called, the function can modify the value of the arguments by using the reference passed in.

This allows us to:

- Modify the value of the function arguments.
- **Avoid making copies of a variable/object for performance reasons.**

The following code shows an example of pass-by-reference. The reference parameters are initialized with the actual arguments when the function is called:

```c++
void swap_num(int &i, int &j) {
 
  int temp = i;
  i = j;
  j = temp;
 
}
 
int main() {
 
  int a = 100;
  int b = 200;
 
  swap_num(a, b);
 
  std::cout << "A is " << a << "\n";
  std::cout << "B is " << b << "\n";
 
}
```

Notice that the `int &i` and `int &j` are the parameters of the function `swap_num()`.

When `swap_num()` is called, the values of the variables `a` and `b` will be modified because they are passed by reference. The output will be:

```
A is 200
B is 100
```

Suppose we didn’t pass-by-reference here and have the parameters as simply `int i` and `int j` in the `swap_num()` function, then `i` and `j` would swap, but `a` and `b` wouldn’t be modified.

And the output will then be:

```
A is 100
B is 200
```

To reiterate, using references as parameters allows us to modify the arguments’ values. This can be very useful in a lot cases.

## Pass-By-Reference with Const

For example, in the following code, we are telling the compiler that the `double` variable `pi` will stay at `3.14` through out the program:

```c++
double const pi = 3.14;
```

If we try to change `pi`, the compiler will throw an error.

Sometimes, we use `const` in a function parameter; this is when we know for a fact that we want to write a function where the parameter won’t change inside the function. Here’s an example:

```c++
int triple(int const i) {
 
  return i * 3;
 
}
```

In this example, we are not modifying the `i`. If inside the function `triple()`, the value of `i` is changed, there will be a compiler error.

So to save the computational cost for a function that doesn’t modify the parameter value(s), we can actually go a step further and use a `const` reference:

```c++
int triple(int const &i) {
 
  return i * 3;
 
}
```

This will ensure the same thing: the parameter won’t be changed. However, by making `i` a reference to the argument, this saves the computational cost of making a copy of the argument.

