# Week 2 — Practice Programs

This file contains small Java programs to reinforce the programming elements covered in Week 2.

---

## 1. Type Casting

### Widening Casting

```java
class WideningCasting {
    public static void main(String[] args) {
        int number = 100;
        double value = number;

        System.out.println(value);
    }
}
```

Output:

```text
100.0
```

Widening conversion happens automatically because `double` can represent an `int` value.

### Narrowing Casting

```java
class NarrowingCasting {
    public static void main(String[] args) {
        double number = 10.75;
        int value = (int) number;

        System.out.println(value);
    }
}
```

Output:

```text
10
```

Narrowing conversion requires explicit casting, and the fractional part is lost.

---

## 2. Operators

```java
class OperatorsDemo {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;

        System.out.println("Addition: " + (a + b));
        System.out.println("Subtraction: " + (a - b));
        System.out.println("Multiplication: " + (a * b));
        System.out.println("Division: " + (a / b));
        System.out.println("Remainder: " + (a % b));

        System.out.println("a > b: " + (a > b));
        System.out.println("a == b: " + (a == b));
    }
}
```

---

## 3. Control Statements

```java
class ControlStatements {
    public static void main(String[] args) {
        int number = 7;

        if (number % 2 == 0) {
            System.out.println("Even");
        } else {
            System.out.println("Odd");
        }

        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }
    }
}
```

This demonstrates:

- `if-else`
- `for` loop
- Modulus operator

---

## 4. break and continue

```java
class BreakContinueDemo {
    public static void main(String[] args) {

        for (int i = 1; i <= 5; i++) {
            if (i == 3) {
                continue;
            }

            System.out.println(i);
        }

        System.out.println("Loop with break:");

        for (int i = 1; i <= 5; i++) {
            if (i == 4) {
                break;
            }

            System.out.println(i);
        }
    }
}
```

### Difference

- `continue` skips the current iteration.
- `break` terminates the loop.

---

## 5. Array Traversal

```java
class ArrayDemo {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};

        for (int number : numbers) {
            System.out.println(number);
        }
    }
}
```

The enhanced `for` loop can be used to traverse an array.

---

## 6. String Comparison

```java
class StringComparison {
    public static void main(String[] args) {
        String a = new String("Java");
        String b = new String("Java");

        System.out.println(a == b);
        System.out.println(a.equals(b));
    }
}
```

Output:

```text
false
true
```

`==` compares object references, while `.equals()` compares the contents of the String objects.

---

## 7. final Keyword

```java
class FinalDemo {
    public static void main(String[] args) {
        final int MAX = 100;

        System.out.println(MAX);

        // MAX = 200; // Compilation error
    }
}
```

A `final` variable cannot be reassigned after initialization.

---

## 8. this Keyword

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void display() {
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
    }

    public static void main(String[] args) {
        Student student = new Student("Arpan", 20);
        student.display();
    }
}
```

`this` refers to the current object.

---

## Key Practice Points

- Widening casting is generally automatic.
- Narrowing casting requires explicit conversion.
- `break` terminates a loop.
- `continue` skips the current iteration.
- Array indexing starts from `0`.
- `==` and `.equals()` behave differently for objects.
- `final` prevents reassignment of a variable.
- `this` refers to the current object.
