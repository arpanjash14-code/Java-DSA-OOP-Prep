# Week 1 — Overview of Object-Oriented Programming and Java

## 1. What is OOP?

Object-Oriented Programming (OOP) is a programming paradigm where programs are designed around objects that contain data and behavior.

- **State** → data/attributes
- **Behavior** → methods

Example:

```java
class Student {
    String name;
    int age;

    void study() {
        System.out.println("Student is studying");
    }
}
```

---

## 2. Four Major OOP Concepts

### Encapsulation

Encapsulation means bundling data and methods together while controlling access to the data.

It is commonly implemented using `private` fields with getter and setter methods.

### Inheritance

Inheritance allows one class to acquire properties and behavior from another class.

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}
```

### Polymorphism

Polymorphism means one interface/name can have multiple forms.

Two important types:

- **Compile-time polymorphism** → Method Overloading
- **Runtime polymorphism** → Method Overriding

### Abstraction

Abstraction means hiding implementation details and exposing only the essential functionality.

It can be achieved using:

- Abstract classes
- Interfaces

---

## 3. Class vs Object

A **class** is a blueprint or template, while an **object** is an instance of that class.

```java
class Car {
    String color;
}

Car car = new Car();
```

Here:

- `Car` → Class
- `car` → Reference variable
- `new Car()` → Creates an object

---

## 4. Java Platform Independence

Java source code is compiled into **bytecode**, which can be executed by a JVM on different platforms.

```text
Java Source Code
       ↓
     javac
       ↓
    Bytecode
       ↓
      JVM
       ↓
Program Execution
```

This is the basis of Java's:

> **Write Once, Run Anywhere**

---

## 5. Important Java Characteristics

Java is commonly described as:

- Object-oriented
- Platform independent
- Simple
- Secure
- Robust
- Portable
- Multithreaded
- Distributed
- Architecture-neutral
- High performance through JIT compilation

---

## 6. JDK, JRE and JVM

### JVM — Java Virtual Machine

JVM executes Java bytecode and provides the environment required to run Java programs.

### JRE — Java Runtime Environment

JRE provides the environment required to run Java applications.

```text
JRE
└── JVM
```

### JDK — Java Development Kit

JDK provides tools required to develop Java applications, including the Java compiler.

```text
JDK
├── Development Tools
└── JRE
    └── JVM
```

Remember:

```text
JDK = JRE + Development Tools
JRE = JVM + Runtime Libraries
```

---

## 7. Is Java Completely Object-Oriented?

**No.**

Java has primitive data types that are not objects:

```text
byte
short
int
long
float
double
char
boolean
```

Java provides wrapper classes when primitive values need to be represented as objects.

Examples:

```text
int     → Integer
char    → Character
double  → Double
boolean → Boolean
```

---

## 8. Important Terms

- Class
- Object
- Reference
- Method
- Constructor
- Encapsulation
- Inheritance
- Polymorphism
- Abstraction
- Interface
- Package
- Exception
- Thread
- JVM
- JRE
- JDK
- Bytecode

---

## Key Takeaways

- OOP is based around objects containing data and behavior.
- The four major OOP concepts are encapsulation, inheritance, polymorphism and abstraction.
- Java source code is compiled into bytecode.
- JVM executes bytecode.
- JRE provides the runtime environment.
- JDK provides development tools.
- Java is not purely object-oriented because it supports primitive data types.
