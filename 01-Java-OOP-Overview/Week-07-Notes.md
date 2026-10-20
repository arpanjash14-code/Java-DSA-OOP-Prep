# Week 7: Abstraction and Interfaces in Java

## 1. What Is Abstraction?

Abstraction is one of the four fundamental pillars of Object-Oriented Programming.

**Definition:** Abstraction means hiding unnecessary implementation details and exposing only the essential features of an object.

For example, when driving a car, we use the steering wheel, accelerator, and brakes without needing to understand the complete internal working of the engine.

In Java, abstraction is commonly achieved using:
1. Abstract classes.
2. Interfaces.

### Advantages

- Reduces complexity.
- Hides implementation details.
- Improves maintainability.
- Supports loose coupling.
- Enables polymorphism.

## 2. Abstract Classes

An abstract class is declared using the `abstract` keyword.

It can contain:
- Abstract methods.
- Concrete methods.
- Instance variables.
- Static variables and methods.
- Constructors.

An abstract class cannot be instantiated directly.

### Example

```java
abstract class Animal {
    abstract void sound();

    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
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
Dog barks
Animal is eating
```

The `Animal` class defines the abstract behavior `sound()`, while `Dog` provides its implementation.

## 3. Abstract Methods

An abstract method is declared without a method body and ends with a semicolon.

```java
abstract void display();
```

Important rules:

1. An abstract method must be declared in an abstract class or an interface.
2. A concrete subclass must implement inherited abstract methods unless it is also declared abstract.
3. Abstract methods cannot be `private`, `static`, or `final`.
4. Constructors cannot be abstract.
5. A class containing an abstract method must be declared abstract.

## 4. Abstract Class Constructors

An abstract class can have constructors. They execute when a concrete subclass object is created.

```java
abstract class Animal {
    Animal() {
        System.out.println("Animal constructor");
    }
}

class Dog extends Animal {
    Dog() {
        System.out.println("Dog constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        new Dog();
    }
}
```

**Output:**
```text
Animal constructor
Dog constructor
```

Although an abstract class cannot be instantiated directly, its constructor initializes the superclass portion of a subclass object.

## 5. Reference of an Abstract Class

An abstract class cannot be instantiated, but a reference of its type can point to a concrete subclass object.

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

The overridden method is selected according to the actual object at runtime.

## 6. What Is an Interface?

An interface defines a contract that implementing classes must fulfill.

It is declared using the `interface` keyword, and a class implements it using `implements`.

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

**Important:** An interface method declared without a body is implicitly `public abstract` unless it is a special method such as a default, static, or private method.

An implementing class must provide a `public` implementation of an abstract interface method.

## 7. Multiple Interfaces

A Java class can implement multiple interfaces.

```java
interface Printable {
    void print();
}

interface Showable {
    void show();
}

class Demo implements Printable, Showable {
    @Override
    public void print() {
        System.out.println("Printing");
    }

    @Override
    public void show() {
        System.out.println("Showing");
    }
}

public class Main {
    public static void main(String[] args) {
        Demo d = new Demo();

        d.print();
        d.show();
    }
}
```

**Output:**
```text
Printing
Showing
```

Java does not support multiple inheritance of classes, but a class can implement multiple interfaces.

## 8. Variables in Interfaces

Variables declared in an interface are implicitly `public static final`.

```java
interface Constants {
    int MAX = 100;
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Constants.MAX);
    }
}
```

**Output:**
```text
100
```

The variable `MAX` is a constant. It cannot be reassigned after initialization.

The declaration is equivalent in its modifiers to:

```java
public static final int MAX = 100;
```

## 9. Default and Static Methods in Interfaces

Modern Java interfaces can contain default and static methods.

### Default Method

A default method has a method body and is inherited by implementing classes unless overridden.

```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle starting");
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
Vehicle starting
```

### Static Method

A static interface method belongs to the interface and is called using the interface name.

```java
interface Utility {
    static void display() {
        System.out.println("Utility method");
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
Utility method
```

A static interface method is not inherited as an ordinary instance method.

## 10. Private Methods in Interfaces

Since Java 9, interfaces can contain private methods to share implementation between their own default or static methods.

```java
interface Greeting {
    private void message() {
        System.out.println("Hello");
    }

    default void greet() {
        message();
    }
}

public class Main {
    public static void main(String[] args) {
        Greeting g = new Greeting() {};
        g.greet();
    }
}
```

**Output:**
```text
Hello
```

Private interface methods cannot be called directly by implementing classes.

For older Java syllabus questions, focus first on abstract methods, constants, and the basic `implements` keyword.

## 11. Abstract Class vs Interface

| Feature | Abstract Class | Interface |
|---|---|---|
| Declaration | `abstract class` | `interface` |
| Object creation | Cannot instantiate directly | Cannot instantiate directly |
| Constructors | Allowed | Not allowed |
| Instance fields | Allowed | No ordinary instance fields |
| Abstract methods | Allowed | Allowed |
| Concrete methods | Allowed | Allowed, including default methods |
| Multiple inheritance | A class extends only one class | A class can implement multiple interfaces |
| Keyword | `extends` | `implements` |

### When to Use Each

Use an abstract class when closely related classes share state or common implementation.

Use an interface when different classes need to follow the same contract, even if they are otherwise unrelated.

## 12. Interface Inheritance

An interface can extend one or more interfaces.

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
```

A concrete class implementing `C` must implement all inherited abstract methods.

```java
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
```

**Exam point:** A class uses `extends` to inherit from a superclass and `implements` to implement interfaces. An interface uses `extends` to inherit from other interfaces.

## 13. Anonymous Classes

An anonymous class is a class without a declared name, commonly used for a one-time implementation.

```java
abstract class Greeting {
    abstract void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting g = new Greeting() {
            @Override
            void sayHello() {
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

Anonymous classes can extend a class or implement an interface without declaring a separate named class.

## 14. Encapsulation vs Abstraction

| Encapsulation | Abstraction |
|---|---|
| Bundles data and methods together | Hides unnecessary implementation details |
| Controls access to internal state | Focuses on essential behavior |
| Commonly uses private fields and controlled methods | Commonly uses interfaces and abstract classes |
| Example: private bank balance | Example: payment interface |

## 15. Important Exam Questions

1. Define abstraction.
2. What is an abstract class?
3. What is an abstract method?
4. Can an abstract class have a constructor?
5. Can an abstract class be instantiated directly?
6. What is an interface?
7. Differentiate between abstract classes and interfaces.
8. Why can a class implement multiple interfaces?
9. What are interface variables?
10. What are default methods and static methods in interfaces?
11. Can an interface extend multiple interfaces?
12. Differentiate between `extends` and `implements`.
13. How does abstraction support polymorphism?
14. Differentiate between encapsulation and abstraction.

## 16. Quick Revision

- Abstraction hides implementation details and exposes essential behavior.
- Abstract classes cannot be instantiated directly.
- Abstract classes can have constructors and concrete methods.
- Concrete subclasses must implement inherited abstract methods.
- Interfaces define contracts.
- Interface variables are implicitly `public static final`.
- Ordinary abstract interface methods are implicitly `public abstract`.
- Implementing classes must implement abstract interface methods as `public`.
- A class can implement multiple interfaces.
- An interface can extend multiple interfaces.
- Interfaces can contain default and static methods.
- Since Java 9, interfaces can contain private methods.
- Use `extends` for class inheritance and interface inheritance.
- Use `implements` for implementing an interface.
