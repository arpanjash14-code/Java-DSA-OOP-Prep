# Week 6: Polymorphism Practice

## 1. Method Overloading

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator c = new Calculator();

        System.out.println(c.add(2, 3));
        System.out.println(c.add(2, 3, 4));
        System.out.println(c.add(2.5, 3.5));
    }
}
```

**Output:**
```text
5
9
6.0
```

**Question:** Why is this an example of compile-time polymorphism?

## 2. Method Overriding

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
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
    }
}
```

**Output:**
```text
Dog barks
```

**Question:** Which implementation of `sound()` executes, and why?

## 3. Dynamic Method Dispatch

```java
class Animal {
    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound();

        a = new Cat();
        a.sound();
    }
}
```

**Output:**
```text
Dog
Cat
```

**Concept:** The overridden instance method is selected according to the actual object at runtime.

## 4. Upcasting

```java
class Parent {
    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void display() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show();

        // p.display(); // Compilation error
    }
}
```

**Output:**
```text
Parent
```

**Question:** Why can't the reference `p` directly call `display()`?

## 5. Downcasting

```java
class Animal {
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();

        if (a instanceof Dog) {
            Dog d = (Dog) a;
            d.bark();
        }
    }
}
```

**Output:**
```text
Barking
```

**Exam point:** An invalid downcast may cause `ClassCastException`.

## 6. The instanceof Operator

```java
class Animal {
}

class Dog extends Animal {
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();

        System.out.println(a instanceof Dog);
        System.out.println(a instanceof Animal);
        System.out.println(a instanceof Object);
    }
}
```

**Output:**
```text
true
true
true
```

**Question:** Why do all three expressions evaluate to `true`?

## 7. Field Hiding vs Method Overriding

```java
class Parent {
    int value = 10;

    void show() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    int value = 20;

    @Override
    void show() {
        System.out.println("Child method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();

        System.out.println(p.value);
        p.show();
    }
}
```

**Output:**
```text
10
Child method
```

**Exam point:**
- Field access depends on the reference type.
- Overridden instance method execution depends on the actual object type.

## 8. Polymorphism with an Abstract Class

```java
abstract class Shape {
    abstract int area();
}

class Rectangle extends Shape {
    int length;
    int width;

    Rectangle(int length, int width) {
        this.length = length;
        this.width = width;
    }

    @Override
    int area() {
        return length * width;
    }
}

public class Main {
    public static void main(String[] args) {
        Shape s = new Rectangle(5, 4);
        System.out.println(s.area());
    }
}
```

**Output:**
```text
20
```

**Question:** Why can't we create an object directly using `new Shape()`?

## 9. Polymorphism with an Interface

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

class Photo implements Printable {
    @Override
    public void print() {
        System.out.println("Printing photo");
    }
}

public class Main {
    public static void main(String[] args) {
        Printable p = new Document();
        p.print();

        p = new Photo();
        p.print();
    }
}
```

**Output:**
```text
Printing document
Printing photo
```

## 10. Output-Based Questions

Try to predict the output before checking the answers.

### Question 1: Overloading

```java
class Demo {
    void show(int x) {
        System.out.println("Integer");
    }

    void show(double x) {
        System.out.println("Double");
    }
}

public class Main {
    public static void main(String[] args) {
        Demo d = new Demo();
        d.show(10);
        d.show(10.5);
    }
}
```

What is the output?

### Question 2: Overriding

```java
class A {
    void display() {
        System.out.println("A");
    }
}

class B extends A {
    @Override
    void display() {
        System.out.println("B");
    }
}

public class Main {
    public static void main(String[] args) {
        A obj = new B();
        obj.display();
    }
}
```

What is the output?

### Question 3: Field Hiding

```java
class A {
    int x = 5;
}

class B extends A {
    int x = 15;
}

public class Main {
    public static void main(String[] args) {
        A obj = new B();
        System.out.println(obj.x);
    }
}
```

What is the output?

### Question 4: Invalid Overloading

Can these methods coexist in the same class?

```java
int calculate(int x) {
    return x;
}

double calculate(int x) {
    return x;
}
```

Explain your answer.

### Question 5: Invalid Downcasting

```java
class Animal {
}

class Dog extends Animal {
}

class Cat extends Animal {
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Cat();
        Dog d = (Dog) a;
    }
}
```

What happens when this code executes?

## 11. Challenge Exercise

Build a simple payment system:

1. Create a `Payment` interface with a `pay(double amount)` method.
2. Implement it in `UPIPayment` and `CardPayment`.
3. Provide different implementations in both classes.
4. Store each object in a `Payment` reference.
5. Call `pay()` and observe the different outputs.

Explain why this is runtime polymorphism.

## Quick Revision

- Overloading: same method name, different parameter lists.
- Overriding: subclass provides a compatible implementation of an inherited instance method.
- Upcasting: subclass object referenced through a superclass type.
- Downcasting: casting a superclass reference to a subclass type.
- `instanceof`: checks runtime type compatibility.
- Fields are hidden, not overridden.
- Static methods are hidden, not overridden.
- Abstract classes cannot be instantiated directly.
- Interfaces enable different classes to share a common contract.
