# Week 5: Inheritance in Java

## 1. What Is Inheritance?

Inheritance is one of the four fundamental pillars of Object-Oriented Programming (OOP).

**Definition:** Inheritance is a mechanism through which one class acquires accessible fields and methods from another class.

- **Superclass / Parent class:** The class whose features are inherited.
- **Subclass / Child class:** The class that inherits from another class.
- **`extends`:** The keyword used for class inheritance in Java.

### Advantages

1. Code reusability.
2. Easier maintenance.
3. Supports method overriding.
4. Enables runtime polymorphism.
5. Helps represent hierarchical relationships between classes.

## 2. Basic Example

```java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
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
Animal is eating
Dog is barking
```

The `Dog` class inherits the accessible `eat()` method from `Animal`.

**Important:** Private members are not directly accessible in a subclass. Constructors are not inherited.

## 3. Types of Inheritance

### 3.1 Single Inheritance

One child class inherits from one parent class.

```java
class A {
    void displayA() {
        System.out.println("Class A");
    }
}

class B extends A {
    void displayB() {
        System.out.println("Class B");
    }
}
```

Diagram:

A → B

### 3.2 Multilevel Inheritance

A class inherits from a class that itself inherits from another class.

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
```

Diagram:

A → B → C

An object of `C` can access the accessible methods inherited from both `A` and `B`.

### 3.3 Hierarchical Inheritance

Multiple child classes inherit from the same parent class.

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

class Cat extends Animal {
    void meow() {
        System.out.println("Meowing");
    }
}
```

Diagram:

```text
      Animal
      /    \
    Dog    Cat
```

### 3.4 Multiple Inheritance Through Classes

Java does not allow a class to extend multiple classes.

The following is invalid:

```java
// Invalid Java syntax
// class C extends A, B { }
```

A class can, however, implement multiple interfaces.

```java
interface Printable {
    void print();
}

interface Showable {
    void show();
}

class Demo implements Printable, Showable {
    public void print() {
        System.out.println("Printing");
    }

    public void show() {
        System.out.println("Showing");
    }
}
```

### 3.5 Hybrid Inheritance

Hybrid inheritance combines two or more inheritance patterns.

Java can represent some hybrid inheritance structures using interfaces, but it does not support arbitrary multiple inheritance of classes.

## 4. The super Keyword

The `super` keyword refers to the immediate superclass part of the current object.

It is commonly used to:
1. Access an accessible superclass field hidden by a subclass field.
2. Call an overridden superclass method.
3. Invoke a superclass constructor.

### 4.1 Accessing a Superclass Field

```java
class Parent {
    int value = 10;
}

class Child extends Parent {
    int value = 20;

    void display() {
        System.out.println(value);
        System.out.println(super.value);
    }
}
```

Output:
```text
20
10
```

### 4.2 Calling a Superclass Method

```java
class Parent {
    void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    @Override
    void display() {
        super.display();
        System.out.println("Child method");
    }
}
```

### 4.3 Calling a Superclass Constructor

```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        super();
        System.out.println("Child constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        new Child();
    }
}
```

Output:
```text
Parent constructor
Child constructor
```

**Exam point:** A superclass constructor executes before the subclass constructor body. If no constructor invocation is written, Java generally inserts an implicit no-argument superclass constructor invocation when permitted.

## 5. Constructor Chaining

Constructor chaining occurs when one constructor invokes another constructor.

- `this()` invokes another constructor in the same class.
- `super()` invokes a constructor of the immediate superclass.

In traditional Java syntax, either invocation must be the first statement in a constructor body. Newer Java versions allow restricted constructor prologues, but standard exam questions commonly use the traditional rule.

Example:

```java
class A {
    A() {
        System.out.println("A");
    }
}

class B extends A {
    B() {
        super();
        System.out.println("B");
    }
}

public class Main {
    public static void main(String[] args) {
        new B();
    }
}
```

Output:
```text
A
B
```

If a parent class has no accessible no-argument constructor, a child constructor must explicitly invoke an appropriate superclass constructor.

## 6. Method Overriding

Method overriding occurs when a subclass provides its own implementation of an inherited instance method with a compatible signature and return type.

Example:

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

Output:
```text
Dog barks
```

### Rules for Overriding

1. The overriding method must have a compatible signature.
2. Its return type must be the same or a permitted covariant return type.
3. It cannot reduce the visibility of the overridden method.
4. A `final` method cannot be overridden.
5. A `private` method is not overridden because it is not inherited as an accessible method.
6. Static methods are hidden, not overridden.
7. The `@Override` annotation lets the compiler check whether the method actually overrides another method.

## 7. Overloading vs Overriding

| Method Overloading | Method Overriding |
|---|---|
| Same method name with different parameter lists | Subclass redefines an inherited instance method |
| Usually occurs in the same class | Requires inheritance |
| Resolved at compile time | Instance method implementation is selected at runtime |
| Return type alone cannot distinguish overloads | Return type must be compatible |

## 8. Runtime Polymorphism and Dynamic Method Dispatch

Runtime polymorphism allows a superclass reference to refer to a subclass object, with an overridden instance method selected according to the actual object type.

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
        Animal a = new Dog();
        a.sound();
    }
}
```

Output:
```text
Dog barks
```

Here:
- Reference type: `Animal`
- Actual object type: `Dog`
- Executed implementation: `Dog.sound()`

This runtime selection of an overridden instance method is called **dynamic method dispatch**.

**Important:** A superclass reference can directly access only members available through its reference type, even when the actual object is a subclass instance. Overridden instance methods are dispatched dynamically.

## 9. The final Keyword and Inheritance

The `final` keyword restricts further modification.

### Final Class

```java
final class A {
}

// Invalid:
// class B extends A { }
```

A final class cannot be subclassed.

### Final Method

```java
class A {
    final void display() {
        System.out.println("Hello");
    }
}
```

A subclass cannot override this method.

### Final Variable

A final variable can be assigned only as permitted by Java's initialization rules and cannot subsequently be reassigned.

**Exam point:** A final class cannot be extended, and a final method cannot be overridden.

## 10. The Object Class

In Java, `Object` is the root of the class hierarchy. Every class without an explicit superclass implicitly extends `Object`.

Important methods include:
- `toString()` — returns a string representation of an object.
- `equals(Object obj)` — tests logical equality according to the implementation.
- `hashCode()` — returns a hash code.
- `getClass()` — returns the runtime class object.

## 11. Inheritance vs Composition

| Inheritance | Composition |
|---|---|
| Represents an "is-a" relationship | Often represents a "has-a" relationship |
| Uses `extends` for classes | One object contains or uses another object |
| Child inherits accessible members | Behavior is reused through contained objects |
| Can create tight coupling | Can offer greater flexibility |

Example:
- A `Dog` **is an** `Animal`.
- A `Car` **has an** `Engine`.

Inheritance should be used when the subclass genuinely represents a specialized form of its superclass.

## 12. Important Exam Questions

1. Define inheritance and explain its advantages.
2. Explain single, multilevel, and hierarchical inheritance.
3. Why does Java not support multiple inheritance through classes?
4. What is the difference between `this` and `super`?
5. What is constructor chaining?
6. Are constructors inherited?
7. What is method overriding?
8. Differentiate between overloading and overriding.
9. Explain runtime polymorphism with an example.
10. What is dynamic method dispatch?
11. What is the purpose of `final` in inheritance?
12. Differentiate between inheritance and composition.

## 13. Quick Revision

- `extends` is used for class inheritance.
- `implements` is used to implement interfaces.
- Java supports single, multilevel, and hierarchical class inheritance.
- A class cannot extend more than one class.
- Constructors are not inherited.
- `super()` invokes a superclass constructor.
- `this()` invokes another constructor in the same class.
- Overriding enables runtime polymorphism.
- Static methods are hidden, not overridden.
- Final methods cannot be overridden.
- Final classes cannot be subclassed.
- `Object` is the root superclass of Java classes.
