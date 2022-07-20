---
title: C++之运算符重载
author: saltycat
date: 2022-07-20 16:11:25 +0800
categories: [C++]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io
---



## Operator Overloading

Operator overloading is an example of C++ polymorphism.  C++ lets you extend operator overloading to user-defined types, permitting you, say, to use the + symbol to add two objects. Again, the compiler uses the number and type of operands to determine which definition of addition to use. Overloaded operators can often make code look more natural.

This simple addition notation conceals the mechanics and emphasizes what is essential, and that is another goal of OOP. 

To overload an operator, you use a special function form called an operator function. An operator function has the following form, where *`op`* is the symbol for the operator being overloaded:

`operator`*`op`*`(argument list)`

## Overloading an operator

In this section, say that we have a class named `Point`. As is named, a `Point` class is used to describe a point, which is defined as below:

```c++
class Point { 
private:
  double x;
  double y;
  double length;
  
public:
  Point(double new_x = 0.0, double new_y = 0.0); 
  double GetX();
  double GetY();
  double GetLength();
  void Modify(double const &new_x, double const &new_y);
 
private:
  double CalcLength(double const &xx, double const &yy);
};


// definitions
double Point::CalcLength(double const &xx, double const &yy) {
  return sqrt(xx * xx + yy * yy);
}

Point::Point(double new_x, double new_y)
 : x(new_x), y(new_y), length(CalcLength(new_x, new_y)) {}

double Point::GetX() { return x; }
double Point::GetY() { return y; }
double Point::GetLength() { return length; }

void Point::Modify(double const &new_x, double const &new_y) {
  x = new_x;
  y = new_y;
  length = CalcLength(new_x, new_y);
}
```

Let's overload `+`, `-`, `*`, `==`, `!=` to this class, 

```c++
#include <iostream>
#include <cmath>
using namespace std;

class Point {
private:
  double x;
  double y;
  double length;
  
public:
  Point(double new_x = 0.0, double new_y = 0.0);
  double GetX();
  double GetY();
  double GetLength();
  void Modify(double const &new_x, double const &new_y);
  Point operator+(Point const &p) const;
  Point operator-(Point const &p) const;
  Point operator*(double k) const;
  bool operator!=(Point const &p) const;
  bool operator==(Point const &p) const;
 
private:
  double CalcLength(double const &xx, double const &yy);
};




double Point::CalcLength(double const &xx, double const &yy) {
  return sqrt(xx * xx + yy * yy);
}

Point::Point(double new_x, double new_y)
 : x(new_x), y(new_y), length(CalcLength(new_x, new_y)) {}

double Point::GetX() { return x; }
double Point::GetY() { return y; }
double Point::GetLength() { return length; }

void Point::Modify(double const &new_x, double const &new_y) {
  x = new_x;
  y = new_y;
  length = CalcLength(new_x, new_y);
}

Point Point::operator+(Point const &t) const {
  Point sum(x + t.x, y + t.y);
  return sum;
}
Point Point::operator-(Point const &t) const {
  Point ret(x - t.x, y - t.y);
  return ret;
}
Point Point::operator*(double k) const {
  Point p(x * k, y * k);
  return p;
}

bool Point::operator!=(Point const &t) const {
  return (x != t.x) || (y != t.y);
}
bool Point::operator==(Point const &t) const {
  return (x == t.x) && (y == t.y);
}

```

The operation functions, like `p1 + p2` (`p1, p2`are both objects of `Point`), are invoked by the 1st object (in this case p1), and take the 2nd object (in this case p2) as an argument. Therefore, it's also OK to use syntax like:

```c++
Point total = p1.operator+(p2);
```

to invoke the operator + of `Point` class.

It is also worth noticing that operator functions defined in a class **always have ONLY one parameter**. If you pursue efficiency, you should use `const Point &t` or `Point const &t` as parameters, because references are more efficient.

> **Caution** 
>
> Don’t return a reference to a local variable or another temporary object. When the function terminates and the local variable or temporary object disappears, the reference becomes a reference to non-existent data.

## Overloading Restrictions

- The overloaded operator must have at least one operand that is a user-defined type.
- You can’t use an operator in a manner that violates the syntax rules for the original operator. Such as using `b = *a`.
- You can’t create new operator symbols.
- You cannot overload the following operators:![cannot be overloaded](/assets/blog_res/2022-07-17-cpp-operator-overloading.assets/image-20220720200703386.png)
- Most of the operators can be overloaded by using either member or nonmember functions. However, you can use only member functions to overload the following operators:![only member functions](/assets/blog_res/2022-07-17-cpp-operator-overloading.assets/image-20220720200836550.png)

![can be be overloaded](/assets/blog_res/2022-07-17-cpp-operator-overloading.assets/image-20220720200917006.png)



## Friends

By making a function a friend to a class, you allow the function the same access privileges that a member function of the class has.

Conceptually, `a = 3 * b;` should be the same as `a = b * 3;`. However `3` is not a `Point` object, that is to say, 3 cannot invoke the operator * which we have defined inside the `Point` class.

We can, however, take the statement `a = 3 * b;` as `a = operator(double const &k, Point const &t);` which is a nonmember function call. Since it's a nonmember function, we don't have access inside the `Point` class. Therefore, we need `friend` to offer us the access to the `Point` class.

Inside the `Point` class declaration, we need to add this declaration:

```c++
friend Point operator*(double k, Point const &p);
```

And outside the class, we need to define it, like this:

```c++
Point operator*(double k, Point const &p) { 
  return p * k;
}
```

Pay attention to that there is **NO** `friend` in the definition. And since it's not a member function of the `Point` class, you **DO NOT** have to add the scope specifier `Point::` in the definition.

With the nonmember overloaded operator function, the left operand of an operator expression corresponds to the first argument of the operator function, and the right operand corresponds to the second argument. 

## Overloading the << Operator

The `ostream` class overloads the operator `<<`, converting it into an output tool. Recall that `cout` is an `ostream` object and that it is smart enough to recognize all the basic C++ types.

So one way to teach `cout` to recognize a Time object is to add a new function operator definition to the `ostream` class declaration. But it’s a dangerous idea to alter the `iostream` file and mess around with a standard interface. Instead, use the Time class declaration to teach the Time class how to use `cout`.

### The First Version

To teach the Time class to use `cout`, you can use a friend function. Why? Because a statement like the following uses two objects, with the `ostream `class object (`cout`) first: `cout << trip;`

If you use a Time member function to overload `<<`, the Time object would come first, as it did when you overloaded the * operator with a member function. That means you would have to use the `<<` operator this way: 

```c++
trip << cout; // if operator<<() were a Time member function
```

Therefore it is extremely reasonable to use a friend function to overload `<<`, like:

```c++
void operator<<(ostream &os, Point const &p) {
  os << "(" << p.x << ", " << p.y << ")";
}
```

The call `cout << trip` should use the `cout` object itself, not a copy, so the function passes the object as a reference instead of by value. Thus, the expression `cout << trip` causes `os` to be an alias for `cout`, and the expression `cerr << trip` causes `os` to be an alias for` cerr`. The Time object can be passed by value or by reference because either form makes the object values available to the function. Again, passing by reference uses less memory and time than passing by value.

### The Second Version

But the first implementation doesn’t allow you to combine the redefined << operator with the ones `cout` normally uses: 

```c++
cout << "Trip time: " << trip << " (Tuesday)\n"; // can't do
```

You can take the same approach with the friend function. You just revise the `operator<<()` function so that it returns a reference to an `ostream` object:

```c++
ostream &operator<<(ostream &os, Point const &p) {
  os << "(" << p.x << ", " << p.y << ")";
  return os;
}
```

Since you cannot overload functions that is only differed in return type, it's highly recommended to use the second version to overload the operator `<<`, because it makes more sense than the first one.
