# Week 5: Inheritance Practice

## 1. Single Inheritance

```java
class Animal {
    void eat() {
        System.out.println("Animal eats");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();
        d.bark();
    }
}
```

**Output:**
```text
Animal eats
Dog barks
```

**Question:** Why can an object of `Dog` call `eat()`?

## 2. Multilevel Inheritance

```java
class A {
    void showA() {
        System.out.println("A");
    }
}

class B extends A {
    void showB() {
        System.out.println("B");
    }
}

class C extends B {
    void showC() {
        System.out.println("C");
    }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
        obj.showA();
        obj.showB();
        obj.showC();
    }
}
```

**Output:**
```text
A
B
C
```

## 3. Hierarchical Inheritance

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("Barking");
    }
}

class Cat extends Animal {
    void sound() {
        System.out.println("Meowing");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        Cat c = new Cat();

        d.eat();
        d.sound();

        c.eat();
        c.sound();
    }
}
```

**Output:**
```text
Eating
Barking
Eating
Meowing
```

## 4. Using the super Keyword

```java
class Parent {
    int value = 10;

    void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    int value = 20;

    void display() {
        System.out.println(value);
        System.out.println(super.value);
        super.display();
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.display();
    }
}
```

**Output:**
```text
20
10
Parent method
```

**Concept:** `super.value` accesses the superclass field hidden by the subclass field.

## 5. Constructor Chaining

```java
class Parent {
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

**Question:** Why does the parent constructor execute first?

## 6. Method Overriding

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

**Concept:** The subclass supplies a new implementation of an inherited instance method.

## 7. Runtime Polymorphism

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

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a;

        a = new Dog();
        a.sound();

        a = new Cat();
        a.sound();
    }
}
```

**Output:**
```text
Dog barks
Cat meows
```

**Concept:** The overridden instance method is selected according to the actual object at runtime.

## 8. Overloading vs Overriding

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator c = new Calculator();

        System.out.println(c.add(2, 3));
        System.out.println(c.add(2, 3, 4));
    }
}
```

**Output:**
```text
5
9
```

**Concept:** The same method name with different parameter lists is overloading, not overriding.

## 9. Final Method

```java
class Parent {
    final void display() {
        System.out.println("Cannot override this method");
    }
}

class Child extends Parent {
    // Overriding display() here would cause a compilation error.
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.display();
    }
}
```

**Output:**
```text
Cannot override this method
```

## 10. Object Class and toString()

```java
class Student {
    int roll;

    Student(int roll) {
        this.roll = roll;
    }

    @Override
    public String toString() {
        return "Student roll: " + roll;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student(101);
        System.out.println(s);
    }
}
```

**Output:**
```text
Student roll: 101
```

**Concept:** `System.out.println(s)` calls the object's `toString()` method to obtain its string representation.

## 11. Output-Based Questions

Try to predict each output before checking the answer.

### Question 1

```java
class A {
    int x = 5;
}

class B extends A {
    int x = 10;

    void show() {
        System.out.println(x);
        System.out.println(super.x);
    }
}
```

What will be printed when `new B().show()` is called?

### Question 2

```java
class A {
    A() {
        System.out.print("A ");
    }
}

class B extends A {
    B() {
        System.out.print("B ");
    }
}

class C extends B {
    C() {
        System.out.print("C ");
    }
}
```

What will be printed when `new C()` is created?

### Question 3

```java
class A {
    void show() {
        System.out.println("A");
    }
}

class B extends A {
    @Override
    void show() {
        System.out.println("B");
    }
}
```

What is the output of this code?

```java
A obj = new B();
obj.show();
```

### Question 4

Can a Java class extend both `A` and `B` using this syntax?

```java
class C extends A, B {
}
```

Explain why or why not.

## 12. Challenge Exercise

Create a small employee hierarchy:

- Create a parent class `Employee` with `name` and `salary`.
- Create a subclass `Manager` with an additional `department` field.
- Initialize fields using constructors.
- Use `super()` to call the parent constructor.
- Override a method named `displayDetails()`.
- Create a `Manager` object and print all its details.

## Quick Revision

- `extends` creates a class inheritance relationship.
- `super()` invokes a superclass constructor.
- `super.method()` invokes a superclass implementation.
- Constructors are not inherited.
- Overriding enables runtime polymorphism.
- Overloading uses different parameter lists.
- Java classes cannot extend multiple classes.
- A final method cannot be overridden.
