# Week 7: Abstraction and Interfaces Practice

## 1. Abstract Class and Abstract Method

```java
abstract class Animal {
    abstract void sound();

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.sound();
        d.eat();
    }
}
```

**Output:**
```text
Barking
Eating
```

**Question:** Why must `Dog` implement `sound()` if it is a concrete class?

## 2. Abstract Class Constructor

```java
abstract class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        new Child();
    }
}
```

**Output:**
```text
Parent constructor
Child constructor
```

**Exam point:** An abstract class cannot be instantiated directly, but its constructor runs when a subclass object is created.

## 3. Reference of an Abstract Class

```java
abstract class Shape {
    abstract int area();
}

class Rectangle extends Shape {
    int length = 5;
    int width = 4;

    @Override
    int area() {
        return length * width;
    }
}

public class Main {
    public static void main(String[] args) {
        Shape s = new Rectangle();
        System.out.println(s.area());
    }
}
```

**Output:**
```text
20
```

## 4. Implementing an Interface

```java
interface Printable {
    void print();
}

class Document implements Printable {
    @Override
    public void print() {
        System.out.println("Printing document");
    }
}

public class Main {
    public static void main(String[] args) {
        Printable p = new Document();
        p.print();
    }
}
```

**Output:**
```text
Printing document
```

**Important:** The implementation of an ordinary abstract interface method must be `public`.

## 5. Implementing Multiple Interfaces

```java
interface A {
    void showA();
}

interface B {
    void showB();
}

class Demo implements A, B {
    @Override
    public void showA() {
        System.out.println("A");
    }

    @Override
    public void showB() {
        System.out.println("B");
    }
}

public class Main {
    public static void main(String[] args) {
        Demo d = new Demo();
        d.showA();
        d.showB();
    }
}
```

**Output:**
```text
A
B
```

## 6. Interface Variables

```java
interface Constants {
    int MAX = 100;
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Constants.MAX);

        // Constants.MAX = 200; // Compilation error
    }
}
```

**Output:**
```text
100
```

**Exam point:** Interface fields are implicitly `public static final`.

## 7. Default Method in an Interface

```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle starts");
    }
}

class Car implements Vehicle {
}

public class Main {
    public static void main(String[] args) {
        Car c = new Car();
        c.start();
    }
}
```

**Output:**
```text
Vehicle starts
```

A class can inherit a default method without providing its own implementation.

## 8. Static Method in an Interface

```java
interface Utility {
    static void display() {
        System.out.println("Static interface method");
    }
}

public class Main {
    public static void main(String[] args) {
        Utility.display();
    }
}
```

**Output:**
```text
Static interface method
```

**Exam point:** Call a static interface method using the interface name.

## 9. Interface Extending Multiple Interfaces

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

interface C extends A, B {
    void methodC();
}

class Demo implements C {
    public void methodA() {
        System.out.println("A");
    }

    public void methodB() {
        System.out.println("B");
    }

    public void methodC() {
        System.out.println("C");
    }
}

public class Main {
    public static void main(String[] args) {
        Demo d = new Demo();
        d.methodA();
        d.methodB();
        d.methodC();
    }
}
```

**Output:**
```text
A
B
C
```

## 10. Anonymous Class

```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting g = new Greeting() {
            @Override
            public void sayHello() {
                System.out.println("Hello!");
            }
        };

        g.sayHello();
    }
}
```

**Output:**
```text
Hello!
```

An anonymous class provides an implementation without declaring a separate named class.

## 11. Output-Based Questions

Predict each output before running the code.

### Question 1

```java
abstract class A {
    A() {
        System.out.print("A ");
    }

    abstract void show();
}

class B extends A {
    B() {
        System.out.print("B ");
    }

    void show() {
        System.out.print("Show");
    }
}

public class Main {
    public static void main(String[] args) {
        B b = new B();
        b.show();
    }
}
```

### Question 2

```java
interface Test {
    int VALUE = 10;
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Test.VALUE);
    }
}
```

What is the output? Can `VALUE` be reassigned?

### Question 3

```java
interface A {
    default void show() {
        System.out.println("A");
    }
}

class B implements A {
}

public class Main {
    public static void main(String[] args) {
        new B().show();
    }
}
```

What is the output?

### Question 4

Can you write this?

```java
abstract class A {
    abstract final void show();
}
```

Explain why or why not.

### Question 5

Can you instantiate an abstract class directly?

```java
abstract class Animal {
}

public class Main {
    public static void main(String[] args) {
        // Animal a = new Animal();
    }
}
```

Explain your answer.

## 12. Challenge Exercise

Create a simple notification system.

Requirements:
1. Create a `Notification` interface with a `send(String message)` method.
2. Implement it in `EmailNotification` and `SMSNotification`.
3. Give each class a different implementation.
4. Store both objects in `Notification` references.
5. Call `send()` on each object.
6. Explain how the program demonstrates abstraction and runtime polymorphism.

## Quick Revision

- Abstract classes cannot be instantiated directly.
- Abstract classes can have constructors and concrete methods.
- Concrete subclasses must implement inherited abstract methods.
- Interface fields are implicitly `public static final`.
- Ordinary abstract interface methods are implicitly `public abstract`.
- Implementations of ordinary abstract interface methods must be public.
- A class can implement multiple interfaces.
- An interface can extend multiple interfaces.
- Default methods have implementations.
- Static interface methods are called using the interface name.
- Anonymous classes provide one-time implementations.
