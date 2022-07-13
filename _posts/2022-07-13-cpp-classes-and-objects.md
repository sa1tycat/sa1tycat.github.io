---
title: C++之类与对象
author: saltycat
date: 2022-07-13 17:51:00 +0800
categories: [C++]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io
---



## Class

The class（类） serves as a blueprint for *objects*（对象）, which are instances（实例） of the class (just like `age` is an instance of `int`). An object gets characteristics and behaviors from its class.

We can create an empty C++ class like this in a header file:



```C++
class City {
 
}; // <-- notice this semicolon!
```

{: file='city.hpp'}



Components of a class are called *class members*（成员）. Just like you can get a string’s length using `.length()`, you can access class members using the dot operator (`object.class_member`).

There are two types of class members:

- *Attributes*（属性）, also known as *member data*（成员数据）, consist of information about an instance of the class.
- *Methods*（方法）, also known as *member functions*（成员函数）, are functions that you can use with an instance of the class. We use a `.` before method names to distinguish them from regular functions.

We *encapsulate*（封装） — or enclose for simpler user access — attributes and methods in a class like this:



```c++
class City {
 
  // attribute
  int population;
 
// we'll explain 'public' later
public:
  // method
  void add_resident() {
    population++;
  }
 
};

```

{: file='city.hpp'}



Unless we have a mostly empty class, it’s common to split function declarations from definitions. We declare methods inside the class (in a header), then define the methods outside the class (in a **.cpp** file of the same name).

How can we define methods outside a class? We can do this using `ClassName::` before the method name to indicate its class like this:



```c++
int City::get_population() {
  return population;
}
```

{: file='city.cpp'}



Unlike with regular functions, **we need to `include` the header file** in the **.cpp** file where we define the methods.

## Objects

An object is an instance of a class, which encapsulates data and functionality pertaining to that data.

To create (or instantiate) an object, we can do this:



```c++
City accra;
```



We can give the object’s attributes values like this (note that these must be attributes you defined in the class and it should be in public):



```C++
accra.population = 2270000;
```



Later, we can access this information using the method we added to the `City` class (if it’s in a `public` part of the class):



```C++
accra.get_population();
```



## Public and Private

By default, everything in a class is `private`, meaning class members are limited to the scope of the class. If you try to access a private class member, you’ll get an error:

```error
error: 'population' is a private member of 'City'
```

But sometimes you need access to class members, and for that, there is `public`. You can use it to make everything below accessible outside the class:



```c++
class City {
 
  int population; 
 
public: // stuff below is public
  void add_resident() { 
    population++;
  }
 
};
```

{: file='city.hpp'}



There is also a `private` access modifier for when you want something below `public` to be private to the class:



```c++
class City {
 
  int population; 
 
public:
  void add_resident() { 
    population++;
  }
 
private: // this stuff is private
  bool is_capital;
 
};
```

{: file='city.hpp'}



## Constructors

A *constructor*（构造函数） is a special kind of method that lets you decide how the objects of a class get created. It has the same name as the class and no return type. Constructors really shine when you want to instantiate an object with specific attributes.

If we want to make sure each `City` is created with a name and a population, we can use parameters and a member initializer list to initialize attributes to values passed in:



```c++
#include "city.hpp"
 
class City {
 
  std::string name;
  int population;
 
public:
  City(std::string new_name, int new_pop);
 
};
```

{: file='city.hpp'}



```C++
City::City(std::string new_name, int new_pop)
  // members get initialized to values passed in 
  : name(new_name), population(new_pop) {}
```

{: file='city.cpp'}



You could also write the definition like this:



```C++
City::City(std::string new_name, int new_pop) {
  name = new_name;
  population = new_pop;
}
```

{: file='city.cpp'}



However, a member initialization list allows you to directly initialize each member when it gets created.

To instantiate an object with attributes, you can do:



```C++
City ankara("Ankara", 5445000);
```

{: file='main.cpp'}



Now we have a `City` called `ankara` with the following attributes:

- `ankara.name` set to `"Ankara"`.
- `ankara.population` set to `5445000`.

## Destructors

A *destructor*（析构函数） is a special method that handles object destruction. Like a constructor, it has the same name as the class and no return type, but is preceded by a `~` operator and takes no parameters:



```c++
class City {
 
  std::string name;
  int population;
 
public:
  City(std::string new_name, int new_pop);
  ~City();
};
 
// city.cpp
City::~City() {
 
  // any final cleanup
 
}
```

{: file='city.hpp'}



Inside, you add any housekeeping that needs to happen before the object is destroyed. You generally won’t need to call a destructor; the destructor will be called automatically in any of the following scenarios:

- The object moves out of scope.
- The object is explicitly deleted.
- When the program ends.

It is worth noticing that a destructor cannot manually be called (the compiler will automatically call it when it needs), and if you didn't create a destructor, the compiler will create one itself.

