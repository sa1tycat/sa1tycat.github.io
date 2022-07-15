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



## Public, Private and Protected

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

C++ provides a third access specifier: `protected`. It works much like `private` does, but allows inherited classes to access those class members.

## Encapsulation

*Encapsulation*（封装） is an important concept in object-oriented programming. It means to hide sensitive details about an object’s implementation（实现） away from the user.

In C++, encapsulation is achieved by declaring class members/attributes as `private` so they cannot be accessed from outside of the class.

However, other parts of the program might need to read or modify the values of private class members. This is where *accessor functions* and *mutator functions* come in.

## Accessor Functions

An *accessor function*（存取函数） (also known as a “getter” function) is a `public` function that returns the value of a `private` member variable:

```C++
class Clock {
private:
  int time = 1200;
 
public:
  // Accessor function for time
  int getTime() {
    return time;
  }
};
 
int main() {
  Clock alarm;
  std::cout << alarm.getTime();    // Output: 1200
}
```

The `main()` function calls the accessor function `getTime()` and gets the value of the `time` attribute even though it is `private`.

**Note**: Accessor methods should always have a return type that matches the type of the member variable they’re accessing.

## Mutator Functions

A *mutator function*（修改函数） (also known as a “setter” function) is a `public` function that sets the value of a `private` member variable:

```C++
class Clock {
private:
  int time = 1200;
 
public:
  // Accessor function for time
  int getTime() {
    return time;
  }
 
  // Mutator function for time
  void setTime(int new_time) {
    time = new_time;
  }
};
 
int main() {
  Clock alarm;
  alarm.setTime(930);
  std::cout << alarm.getTime();    // Output: 930
}
```

Mutator functions usually have a return type of `void`. They are only responsible for resetting the value of an existing class member. As such, mutator functions often have one parameter of the same type as the attribute.

## Constructors

A *constructor*（构造函数） is a special kind of method that lets you decide how the objects of a class get created. It has the same name as the class and no return type. Constructors really shine when you want to instantiate an object with specific attributes.

### Default Constructors

A *default* constructor is a constructor that takes no parameters. If the object is created without specifying the initialization values, the default constructor will be called.

In this example, the `House` class has a default constructor:

```c++
class House {
private:
  std::string location;
  int rooms;
 
public:
  // Default constructor
  House() {
    location = "New York";
    rooms = 5;
  }
 
  void summary() {
    std::cout << location << " house with " << rooms << " rooms.\n";
  }
};
```

This class has two `private` attributes that are initialized by the default `House()` constructor. The default constructor will be invoked when an object of the `House` class is created:

```c++
int main() {
  House red_house;    // Calls House() default constructor
 
  red_house.summary();
 
  return 0;
}
```

The values of the `location` and `rooms` attributes will be set according to the code inside the default constructor. The program above produces the output:

```
New York house with 5 rooms.
```

A default constructor is great for ensuring that an object of the class is always initialized appropriately.

#### Define Constructors Outside of Class

Just like methods, constructors can be defined outside of their class definition. Use the `ClassName::` namespace to indicate that the constructor belongs to that class.

For example, if the `House()` constructor was defined separately, it would look like this:

```c++
House::House() {
  location = "New York";
  rooms = 5;
}
```

This syntax applies to other types of constructors as well.

### Constructor with Parameters

Similar to functions, constructors can be declared with parameters. Constructors with parameters allow objects to be instantiated with specific values.

Let’s add a constructor to the `House` class that takes two parameters:

```c++
class House {
private:
  std::string location;
  int rooms;
 
public:
  // Default constructor
  House() {
    location = "New York";
    rooms = 5;
  }
 
  // Constructor with parameters
  House(std::string loc, int num) {
    location = loc;
    rooms = num;
  }
 
  void summary() {
    std::cout << location << " house with " << rooms << " rooms.\n";
  }
};
```

To use this new constructor with parameters, create the object and provide the two required arguments:

```c++
int main() {
  House green_house("Boston", 3);    // Calls House(std::string, int) constructor
 
  green_house.summary();
 
  return 0;
}
```

This program will print:

```
Boston house with 3 rooms.
```

**Note**: Notice that there are two constructors inside the `House` class definition. There is no conflict between them because *function overloading* allows multiple constructors to co-exist in a class as long as each has a unique number and type of parameters.



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

### Constructor Default Parameters

The parameters of a constructor can have default values. Defaulting the constructor parameters helps keep the number of constructors small.

Using default parameters, the two constructors in the `House` class can be combined into one:

```c++
class House {
private:
  std::string location;
  int rooms;
 
public:
  // Constructor with default parameters
  House(std::string loc = "New York", int num = 5) {
    location = loc;
    rooms = num;
  }
 
  void summary() {
    std::cout << location << " house with " << rooms << " rooms.\n";
  }
};
```

This constructor acts as both a default constructor and a constructor with parameters; it can accept one or two user-provided values or no values at all.

```C++
int main() {
  House default_house;    // Calls House("New York", 5), default
  House texas_house("Texas");    // Calls House("Texas", 5)
  House big_florida_house("Florida", 10);    // Calls House("Florida", 10)
}
```

**Note**: When calling a constructor with multiple default parameters, the compiler will match the arguments starting from the leftmost parameter. Therefore, a constructor call like this is not possible because it skips over the first parameter:

```C++
House big_house(10);    // Error: no constructor to handle House(int)
```

To make this possible, a constructor that only takes one `int` parameter needs to be added to the class definition.

### Member Initializer Lists

Strictly speaking, the constructor examples shown so far have been **assigning**（赋） value to attributes, not **initiating**（初始化） them. This subtle difference comes into play when dealing with `const` or reference variables, which must be initialized when they are declared. Using the regular assignment syntax for such types of attributes will not work:

```c++
class Book {
private:
  const std::string title;
  const int pages;
public:
  Book() {
    title = "Diary";    // Error: const variables can't be assigned to
    pages = 100;    // Error: const variables can't be assigned to
  };
```

To solve this problem, C++ provides a way to initialize attributes via a *member initializer list*（成员初值列表）. It is placed after the constructor parameters list. The member initializer list begins with a colon `:`, and then lists each attribute and the initial value for that attribute. Each attribute in the list is separated by a comma.

Let’s rewrite the `Book()` constructor using the member initializer list:

```C++
class Book {
private:
  const std::string title;
  const int pages;
public:
  Book() 
    : title("Diary"), pages(100) {}    // Member initializer list
};
```

This constructor will now work as intended because the member initializer list will properly initialize the `const` attributes instead of assign values to them.

**Note**: Don’t forget to add the brackets `{}` after the member initializer list! Any code can still be placed into the constructor body.

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



```c++
City::~City() {
 
  // any final cleanup
 
}
```

{: file='city.cpp'}



Inside, you add any housekeeping that needs to happen before the object is destroyed. You generally won’t need to call a destructor; the destructor will be called automatically in any of the following scenarios:

- The object moves out of scope.
- The object is explicitly deleted.
- When the program ends.

It is worth noticing that a destructor cannot manually be called (the compiler will automatically call it when it needs), and if you didn't create a destructor, the compiler will create one itself.

# Inheritance

In C++, *inheritance*（继承） is the concept of defining a class in terms of another class. Inheritance makes it possible to reuse code and to establish hierarchical（等级制） relationships between classes.

### Basic Inheritance

Classes that belong to an inheritance relationship are separated into two groups:

- *Base class*（基类） or *parent class*: The class being inherited from.
- *Derived class*（派生类） or *child class*: The class that inherits from the base class.

A derived class inherits attributes and methods from its base class. To declare a derived class, place `:` after the class name and then write the access specifier and the name of the base class:

```C++
#include <iostream>
 
// Base class
class Animal {
public:
  std::string gender;
  int age;
};
 
// Derived class
class Dog: public Animal  {
public:
  std::string breed;
 
  void sound() {
    std::cout << "Woof\n";
  }
};
 
int main() {
  Dog buddy;
  buddy.gender = "male";
  buddy.age = 8;
  buddy.breed = "husky";
 
  buddy.sound(); // Outputs: Woof
 
  return 0;
}
```

In the example above, the `Dog` class inherits the attributes `gender` and `age` from the `Animal` class. The `Dog` class also has a new `breed` attribute and a new `sound()` function of its own.

#### Constructor Inheritance

For the sake of simplicity, the class attributes in the previous example were kept as `public`. Normally, class attributes should be `private` because of *encapsulation*.

A derived class also inherits the constructors of its base class. A derived class can initialize the `private` attributes of its base class by using the base class constructor in a member initializer list:

```c++
#include <iostream>
 
// Base class
class Animal {
private:
  std::string gender;
  int age;
 
public:
  Animal(std::string new_gender, int new_age)
    : gender(new_gender), age(new_age) {}
};
 
// Derived class
class Dog: public Animal  {
private:
  std::string breed;
 
public:
  // Call base class constructor
  Dog(std::string new_gender, int new_age, std::string new_breed)
    : Animal(new_gender, new_age), breed(new_breed) {}
 
  void sound() {
    std::cout << "Woof\n";
  }
};
 
int main() {
  // Calls Dog(string, int, string) constructor
  Dog buddy("male", 8, "Husky");
 
  // Output: Woof
  buddy.sound();
 
  return 0;
}
```

### Multilevel Inheritance（多层继承）

A class can inherit from another class that is already derived from another class. This concept is sometimes referred to as an *inheritance chain*（继承链）:

```c++
#include <iostream>
 
class A {    // A is the base class
public:
  int a;
 
  A() { std::cout << "Constructing A\n"; }
};
 
class B: public A {    // class B inherits from class A
public:
  int b;
 
  B() { std::cout << "Constructing B\n"; }
};
 
class C: public B {    // class C inherits from class B
public:
  int c;
 
  C() { std::cout << "Constructing C\n"; }
};
 
int main() {
  C example; 
 
  return 0;
}
```

In an inheritance chain, the “most base” class is always constructed first. The order of class construction then goes from the parent to the child. The output of the above program is:

```
Constructing A
Constructing B
Constructing C
```

In an inheritance chain, a derived class inherits from all the base classes before it. Class `C` has all three attributes of `a`, `b`, and `c` because it is the last class in the inheritance chain.

### Types of Inheritance

When declaring a derived class, the base class may be inherited through three different types of inheritance:

- Public Inheritance: The access specifiers of the base class members stay the same in the derived class. This is the most commonly used type of inheritance.
- Protected Inheritance: `public` and `protected` members of the base class become `protected` members of the derived class.
- Private Inheritance: All base class members become `private` members of the derived class.

The following table is a reminder of the three access specifiers in C++:

| Access                 | `public` | `protected` | `private` |
| ---------------------- | -------- | ----------- | --------- |
| Inside the class       | yes      | yes         | yes       |
| Inside derived classes | yes      | yes         | no        |
| Outside the class      | yes      | no          | no        |

# Polymorphism

The term *polymorphism*（多态） means “many forms”. In C++, polymorphism applies to class methods in an inheritance relationship.

Polymorphism allows a derived class to override methods inherited from its base class. Although they have the same function signature, the C++ compiler will resolve function execution depending on the type of object that invokes the function.

Take a look at the following example where two classes inherit from a common base class:

```c++
#include <iostream>
 
class Animal {
public:
  void action() {
    std::cout << "The animal does something.\n";
  }
};
 
class Fish: public Animal {
public:
  void action() {
    std::cout << "Fish swim.\n";
  }
};
 
class Bird: public Animal {
public:
  void action() {
    std::cout << "Birds fly.\n";
  }
};
 
int main() {
  Animal newAnimal;
  Fish newFish;
  Bird newBird;
 
  newAnimal.action();
  newFish.action();
  newBird.action();
 
  return 0;
}
```

The above program will produce the output:

```
The animal does something.
Fish swim.
Birds fly.
```

The two derived classes `Fish` and `Bird` both inherit the `action()` method from the base class `Animal`. However, each class has its own unique implementation of this method.

In other words, the `action()` method has “polymorphed” into three different forms. The type of the caller object determines which form of `action()` is executed.
