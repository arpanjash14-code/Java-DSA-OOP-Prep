# Week 2 — Java Programming Elements

## 1. Java Program Structure

A basic Java program consists of a class and one or more methods.

```java
class Hello {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

### Important parts

- `class` → defines a class
- `main()` → entry point of a Java application
- `public` → accessible from outside the class
- `static` → can be called without creating an object
- `void` → method does not return a value
- `String[] args` → command-line arguments

---

## 2. Tokens in Java

A **token** is the smallest individual element of a Java program.

Major types of tokens include:

1. Keywords
2. Identifiers
3. Literals
4. Operators
5. Separators

Example:

```java
int age = 20;
```

Here:

- `int` → keyword
- `age` → identifier
- `20` → literal
- `=` → operator
- `;` → separator

---

## 3. Keywords

Keywords are reserved words that have predefined meanings in Java.

Examples:

```text
class
public
private
static
void
int
new
if
else
for
while
return
extends
implements
try
catch
throw
throws
final
abstract
interface
```

Keywords cannot be used as identifiers.

---

## 4. Identifiers

Identifiers are names given to programming elements such as:

- Classes
- Variables
- Methods
- Objects
- Packages

Example:

```java
class Student {
    int age;

    void displayAge() {
        System.out.println(age);
    }
}
```

Here:

- `Student` → identifier
- `age` → identifier
- `displayAge` → identifier

### Identifier Rules

- Can contain letters, digits, `_` and `$`
- Cannot start with a digit
- Cannot contain spaces
- Cannot be a Java keyword
- Java identifiers are case-sensitive

Valid:

```text
student
studentAge
_student
$amount
```

Invalid:

```text
2student
student age
class
```

---

## 5. Variables

A variable is a named memory location used to store data.

Example:

```java
int age = 20;
```

### Types of Variables

#### Local Variable

Declared inside a method, constructor or block.

```java
void display() {
    int age = 20;
}
```

#### Instance Variable

Declared inside a class but outside methods.

```java
class Student {
    int age;
}
```

Each object can have its own copy.

#### Static Variable

Declared using the `static` keyword.

```java
class Student {
    static String college = "NIT";
}
```

A static variable belongs to the class rather than individual objects.

---

## 6. Data Types

Java data types are divided into two major categories:

```text
Data Types
├── Primitive
└── Reference
```

### Primitive Data Types

Java has 8 primitive data types:

| Type | Typical Size | Example |
|---|---:|---|
| byte | 8-bit | `10` |
| short | 16-bit | `1000` |
| int | 32-bit | `100000` |
| long | 64-bit | `100000L` |
| float | 32-bit | `3.14f` |
| double | 64-bit | `3.14` |
| char | 16-bit | `'A'` |
| boolean | JVM-dependent | `true` |

Example:

```java
int age = 20;
double salary = 25000.50;
char grade = 'A';
boolean passed = true;
```

### Reference Types

Reference variables store references to objects.

Examples:

```java
String name = "Arpan";
Student student = new Student();
int[] numbers = {1, 2, 3};
```

---

## 7. Literals

A literal is a fixed value written directly in a Java program.

Examples:

```java
int a = 10;
double b = 3.14;
char c = 'A';
String s = "Java";
boolean flag = true;
```

Common literal types include:

- Integer literals
- Floating-point literals
- Character literals
- String literals
- Boolean literals
- `null` literal

---

## 8. Type Casting

Type casting converts a value from one data type to another.

### Widening Conversion

Smaller compatible type → larger compatible type.

```java
int x = 10;
double y = x;
```

This happens automatically.

```text
byte → short → int → long → float → double
```

### Narrowing Conversion

Larger type → smaller type.

It requires explicit casting.

```java
double x = 10.5;
int y = (int) x;
```

The fractional part is lost.

---

## 9. Operators

Java provides several categories of operators.

### Arithmetic Operators

```text
+
-
*
/
%
```

Example:

```java
int a = 10;
int b = 3;

System.out.println(a + b);
System.out.println(a % b);
```

### Relational Operators

```text
==
!=
>
<
>=
<=
```

These produce a boolean result.

### Logical Operators

```text
&&
||
!
```

Example:

```java
if (age >= 18 && citizen) {
    System.out.println("Eligible");
}
```

### Assignment Operators

```text
=
+=
-=
*=
/=
%=
```

### Unary Operators

```text
++
--
+
-
!
```

### Conditional Operator

The ternary operator:

```java
int max = (a > b) ? a : b;
```

---

## 10. Operator Precedence

When an expression contains multiple operators, Java follows operator precedence rules.

For example:

```java
int result = 10 + 5 * 2;
```

Multiplication is evaluated before addition.

Therefore:

```text
10 + 5 * 2
= 10 + 10
= 20
```

Parentheses can be used to explicitly control evaluation:

```java
int result = (10 + 5) * 2;
```

Result:

```text
30
```

---

## 11. Control Statements

Control statements determine the flow of execution.

### if-else

```java
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```

### switch

```java
switch (day) {
    case 1:
        System.out.println("Monday");
        break;

    case 2:
        System.out.println("Tuesday");
        break;

    default:
        System.out.println("Invalid day");
}
```

### for Loop

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

### while Loop

```java
while (condition) {
    // statements
}
```

### do-while Loop

```java
do {
    // statements
} while (condition);
```

The `do-while` loop executes its body at least once.

---

## 12. break, continue and return

### break

Terminates the nearest loop or switch.

```java
for (int i = 0; i < 10; i++) {
    if (i == 5)
        break;
}
```

### continue

Skips the current iteration and continues with the next iteration.

```java
for (int i = 0; i < 5; i++) {
    if (i == 2)
        continue;

    System.out.println(i);
}
```

### return

Terminates a method and optionally returns a value.

```java
int add(int a, int b) {
    return a + b;
}
```

---

## 13. Arrays

An array stores multiple values of the same type.

```java
int[] numbers = {10, 20, 30, 40};
```

Array indexing starts from `0`.

```java
System.out.println(numbers[0]);
```

Output:

```text
10
```

The size of an array is fixed after creation.

```java
int[] numbers = new int[5];
```

---

## 14. Strings

`String` is a class in Java.

```java
String name = "Arpan";
```

Strings are objects and are immutable.

Example:

```java
String s = "Java";
s = s + " Programming";
```

A new String object may be created because String objects cannot be modified after creation.

---

## 15. Important Java Keywords

### final

`final` can be used with variables, methods and classes.

```java
final int MAX = 100;
```

A final variable cannot be reassigned.

A final method cannot be overridden.

A final class cannot be inherited.

### this

`this` refers to the current object.

```java
class Student {
    int age;

    Student(int age) {
        this.age = age;
    }
}
```

### new

`new` is used to create objects.

```java
Student s = new Student();
```

---

## 16. Important Exam Points

- Java is case-sensitive.
- Java source files normally use the `.java` extension.
- Bytecode is stored in `.class` files.
- `javac` compiles Java source code.
- JVM executes bytecode.
- Java has 8 primitive data types.
- Array indexing starts from `0`.
- Array size is fixed after creation.
- `String` is a class and String objects are immutable.
- `==` compares primitive values and compares references when used with objects.
- `.equals()` is commonly used to compare the contents of objects such as Strings.
- `break` terminates a loop or switch.
- `continue` skips the current iteration.
- `return` exits a method.
- `final` prevents reassignment, overriding or inheritance depending on its usage.

---

## Quick Revision

```text
Java Program
    ↓
Class
    ↓
Methods + Variables
    ↓
Statements + Expressions
    ↓
Operators
    ↓
Control Flow
    ↓
Objects and Data
```

### Key Takeaways

- Tokens are the basic elements of a Java program.
- Variables store values or references.
- Java has primitive and reference data types.
- Operators are used to perform operations on values.
- Control statements determine program flow.
- Arrays store multiple values of the same type.
- Strings are immutable objects.
- Type casting can be widening or narrowing.
