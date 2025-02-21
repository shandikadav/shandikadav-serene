+++
title = "Introduction to Dart Object-Oriented Programming (OOP)"
date = 2024-12-26
draft = false

[taxonomies]
categories = [ "2024" ]
tags = [ "programming", "dart", "flutter", "oop" ]
+++

## What is Object Oriented Programming?

OOP, or Object Oriented Programming, is a programming paradigm focused on structuring programs around **objects** that have specific properties and behaviors. These objects can interact with one another to achieve a particular goal.

OOP is especially essential for managing high-complexity programs. By applying OOP principles, programs can become more efficient. To implement OOP effectively, you need to understand its core components: **Object**, **Class**, **Property**, **Method**, and **Constructor**. These will be discussed one by one in this module.

---

## Object

An **object** is an instance of a class. It has attributes (variables) representing its state and methods (functions) for interacting with it.

For example, imagine creating a company. Each company, such as *"Apple Inc."* or *"Microsoft Corporation"*, is an object (or instance) of the *Company* class. Each object has unique values for attributes like address and number of employees.

---

## Class

A **class** serves as a blueprint or template. It defines the properties (variables) and methods (functions) an object will have.

For instance, the *Company* class might define attributes like company name, address, and employee count, as well as methods for managing company information.

---

## Defining Classes and Objects

To define a class in Dart, use the `class` keyword followed by the class name.

```dart
class Person {
}
```

To create an object, call the class name followed by parentheses.
```dart
class Person {
  String name = 'John Doe';
  int age = 21;
  String? address;
}

var person1 = Person();
```

To access a field property:
```dart
print(person1.name);
print(person1.age);
print(person1.address);
```

## Adding Methods

You can also add methods to a class. A method is a function within a class. It can include parameters and return values.
```dart
void sayHi(String name) {
  print('Hello, $name');
}
```

Accessing methods is similar to accessing properties:

```dart
person1.sayHi('John');
```

## Constructor
In Dart, a constructor is a special method used to create and initialize an object. Constructors set initial property values and handle tasks necessary when an object is created.

### Characteristics of a Constructor:
- It shares the same name as the class.
- It does not have a return type.
- It is automatically invoked when an object is created.
- If no constructor is defined, a default no-argument constructor is provided.

Example:
```dart
class Person {
  String name = 'John Doe';
  String? address;
  final String country = 'Indonesia';

  Person(String paramName, String paramAddress) {
    name = paramName;
    address = paramAddress;
  }
}

void main() {
  var person = Person('Shandika David Ardiansyah', 'Jogja');

  print(person.name);
  print(person.address);
}
```
## this Keyword

The this keyword refers to the current instance of the class. It helps differentiate between class fields and method parameters with the same name.

```dart
Person(String name, String address) {
  this.name = name;
  this.address = address;
}
```

## Initializing Formal Parameters

In Dart, you can simplify parameter initialization using **formal parameter initialization**:
```dart
Person(this.name, this.address);
```
