# Week 6: Polymorphism in Java

## 1. What Is Polymorphism?

Polymorphism is one of the four fundamental pillars of Object-Oriented Programming (OOP).

**Definition:** Polymorphism means "many forms." In Java, it allows the same method name or interface to represent different behaviors in different situations.

The two main types are:

1. Compile-time polymorphism — method overloading.
2. Runtime polymorphism — method overriding and dynamic method dispatch.

## 2. Compile-Time Polymorphism: Method Overloading

Method overloading occurs when multiple methods in the same class have the same name but different parameter lists.

The parameter lists can differ in:
- Number of parameters.
- Types of parameters.
- Order of parameter types.

### Example

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

The compiler determines which overloaded method to call based on the arguments.

### Important Rules

- Overloaded methods must have different parameter lists.
- Changing only the return type does not create a valid overload.
- Changing only parameter names does not create an overload.
- Overloading can occur in the same class or across inherited methods.

Invalid example:

```java
// Invalid: methods differ only by return type.
int calculate(int x) {
    return x;
}

// double calculate(int x) {
//     return x;
// }
```

## 3. Runtime Polymorphism: Method Overriding

Method overriding occurs when a subclass provides a compatible implementation of an inherited instance method.

### Example

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
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

**Output:**
```text
Dog barks
```

Although the reference type is `Animal`, the actual object is `Dog`. Java selects the overridden instance method at runtime.

### Rules for Overriding

1. The method signature must be compatible with the inherited method.
2. The return type must be the same or a permitted covariant return type.
3. The overriding method cannot reduce access visibility.
4. A final method cannot be overridden.
5. Private methods are not overridden because they are not inherited as accessible methods.
6. Static methods are hidden rather than overridden.
7. The `@Override` annotation helps the compiler detect mistakes.

## 4. Dynamic Method Dispatch

Dynamic method dispatch is the mechanism through which Java selects an overridden instance method at runtime according to the actual object.

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

Remember:
- Reference type: `Animal`.
- Actual object type: `Dog` or `Cat`.
- Method implementation: selected at runtime for overridden instance methods.

## 5. Upcasting

Upcasting means treating a subclass object as an instance of its superclass.

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

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.eat();

        // a.bark(); // Compilation error
    }
}
```

Upcasting is automatic and safe.

The reference can directly access only members available through its declared type. To call `bark()`, the code would need a suitable downcast or a redesign using an appropriate common interface.

## 6. Downcasting

Downcasting means converting a superclass reference into a subclass reference.

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

A cast does not change the actual object. It changes the reference type used to access it.

**Important:** An invalid downcast can throw `ClassCastException`. Check the actual type before casting when necessary.

## 7. The instanceof Operator

The `instanceof` operator checks whether an object is compatible with a given type.

```java
Animal a = new Dog();

System.out.println(a instanceof Dog);
System.out.println(a instanceof Animal);
```

**Output:**
```text
true
true
```

It is commonly used before downcasting.

`null instanceof SomeClass` evaluates to `false`.

## 8. Overloading vs Overriding

| Feature | Overloading | Overriding |
|---|---|---|
| Meaning | Same method name, different parameter lists | Subclass provides a new implementation |
| Inheritance required | No | Yes |
| Method selection | Compile time | Runtime for overridden instance methods |
| Parameters | Must differ | Must be compatible with inherited signature |
| Return type alone | Cannot distinguish overloads | Must be compatible |
| Static methods | Can be overloaded | Are hidden, not overridden |

## 9. Polymorphism with Abstract Classes

An abstract class can define common behavior and require subclasses to implement abstract methods.

```java
abstract class Shape {
    abstract double area();
}

class Circle extends Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    double length;
    double width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double area() {
        return length * width;
    }
}

public class Main {
    public static void main(String[] args) {
        Shape s = new Circle(2);
        System.out.printf("%.2f%n", s.area());

        s = new Rectangle(4, 5);
        System.out.printf("%.2f%n", s.area());
    }
}
```

**Output:**
```text
12.57
20.00
```

A common superclass reference can represent different subclass objects and invoke their respective implementations.

## 10. Polymorphism with Interfaces

An interface specifies a contract that implementing classes fulfill.

```java
interface Payment {
    void pay(double amount);
}

class UpiPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid using UPI: " + amount);
    }
}

class CardPayment implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paid using card: " + amount);
    }
}

public class Main {
    public static void main(String[] args) {
        Payment p = new UpiPayment();
        p.pay(500);

        p = new CardPayment();
        p.pay(1000);
    }
}
```

**Output:**
```text
Paid using UPI: 500.0
Paid using card: 1000.0
```

Interfaces are useful for polymorphism because different classes can provide different implementations of the same contract.

## 11. Can Fields Be Polymorphic?

Fields do not behave like overridden instance methods.

```java
class Parent {
    int value = 10;
}

class Child extends Parent {
    int value = 20;
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();
        System.out.println(p.value);
    }
}
```

**Output:**
```text
10
```

Field access is determined by the reference's declared type. The subclass field hides the superclass field; it does not override it.

## 12. Common Exam Questions

1. Define polymorphism.
2. Explain compile-time and runtime polymorphism.
3. What is method overloading?
4. What is method overriding?
5. What is dynamic method dispatch?
6. Differentiate between overloading and overriding.
7. What is upcasting?
8. What is downcasting?
9. Explain the `instanceof` operator.
10. Can static methods be overridden?
11. Can methods be overloaded by changing only their return types?
12. How do interfaces support polymorphism?
13. Does field access use runtime polymorphism?
14. What happens if an invalid downcast is performed?

## 13. Quick Revision

- Polymorphism means many forms.
- Overloading is compile-time polymorphism.
- Overriding enables runtime polymorphism.
- `@Override` helps verify an override.
- Dynamic method dispatch selects an overridden instance method at runtime.
- Upcasting converts a subclass reference to a superclass reference.
- Downcasting converts a superclass reference to a subclass reference.
- `instanceof` checks type compatibility.
- Static methods are hidden, not overridden.
- Fields are hidden, not overridden.
- Interfaces and abstract classes are commonly used to achieve polymorphism.
