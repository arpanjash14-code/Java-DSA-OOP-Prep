# Week 8: Packages and Exception Handling in Java

# Part A: Packages in Java

## 1. What Is a Package?

A package is a mechanism for grouping related classes, interfaces, and other types.

Packages help with:
- Organizing code.
- Avoiding naming conflicts.
- Controlling access.
- Improving maintainability.

Java provides built-in packages such as:
- `java.lang`
- `java.util`
- `java.io`
- `java.net`
- `java.sql`

## 2. Creating a User-Defined Package

The `package` statement declares the package to which a source file belongs.

Example:

```java
package mypack;

public class Demo {
    public void display() {
        System.out.println("Hello from mypack");
    }
}
```

Save this file as `Demo.java` inside the `mypack` directory.

A second file can import and use the class:

```java
import mypack.Demo;

public class Main {
    public static void main(String[] args) {
        Demo d = new Demo();
        d.display();
    }
}
```

**Output:**
```text
Hello from mypack
```

**Exam point:** The package declaration, when present, must precede import declarations and type declarations.

## 3. The import Keyword

The `import` keyword allows a type or static member to be referred to by a simpler name.

Examples:

```java
import java.util.Scanner;
import java.util.ArrayList;
```

Importing all accessible types from a package:

```java
import java.util.*;
```

Important points:
- `java.lang` is imported automatically.
- Importing `java.util.*` does not import subpackages such as `java.util.concurrent`.
- Imports do not copy classes into your program.
- A fully qualified class name can be used without an import.

Example:

```java
public class Main {
    public static void main(String[] args) {
        java.util.Scanner sc =
            new java.util.Scanner(System.in);

        int n = sc.nextInt();
        System.out.println(n);

        sc.close();
    }
}
```

## 4. Access Control and Packages

Java has four commonly taught access levels.

| Modifier | Same Class | Same Package | Subclass in Another Package | Unrelated Class in Another Package |
|---|---|---|---|---|
| `private` | Yes | No | No | No |
| Default (package-private) | Yes | Yes | No, not through package access | No |
| `protected` | Yes | Yes | Yes, subject to subclass access rules | No |
| `public` | Yes | Yes | Yes | Yes |

Important points:
- `private` members are accessible within their declaring class.
- Package-private members are accessible within the same package.
- `protected` members are accessible within the same package and in subclasses under Java's cross-package access rules.
- `public` members are accessible wherever their declaring type is accessible.
- A top-level class can normally be `public` or package-private, not `private` or `protected`.

## 5. Static Import

Static import allows static members to be used without qualifying them with the declaring type.

```java
import static java.lang.Math.sqrt;

public class Main {
    public static void main(String[] args) {
        System.out.println(sqrt(25));
    }
}
```

**Output:**
```text
5.0
```

# Part B: Exception Handling

## 6. What Is an Exception?

An exception is an event represented by an object that interrupts the normal flow of a program.

Exception handling helps a program respond to certain runtime problems.

Examples include:
- Dividing an integer by zero.
- Accessing an array using an invalid index.
- Reading a missing file.
- Converting invalid text into a number.

Java's exception hierarchy begins with `Throwable`.

```text
Object
  └── Throwable
       ├── Error
       └── Exception
            └── RuntimeException
```

`Error` generally represents serious problems that applications are not normally expected to handle.

## 7. Types of Exceptions

### 7.1 Checked Exceptions

Checked exceptions are checked by the compiler at compile time.

They must generally be caught or declared using `throws`.

Examples:
- `IOException`
- `SQLException`
- `ClassNotFoundException`

### 7.2 Unchecked Exceptions

Unchecked exceptions include subclasses of `RuntimeException`.

They are not subject to the same catch-or-declare requirement.

Examples:
- `ArithmeticException`
- `NullPointerException`
- `ArrayIndexOutOfBoundsException`
- `NumberFormatException`

### Checked vs Unchecked

| Checked Exceptions | Unchecked Exceptions |
|---|---|
| Checked by the compiler | Not subject to compile-time catch-or-declare checking |
| Usually subclasses of `Exception` other than `RuntimeException` | Subclasses of `RuntimeException` |
| Must generally be caught or declared | Catching or declaring is not compulsory |
| Example: `IOException` | Example: `NullPointerException` |

## 8. try and catch

The `try` block contains code that may throw an exception.

A `catch` block handles a matching exception.

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
            System.out.println(result);
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero");
        }
    }
}
```

**Output:**
```text
Cannot divide by zero
```

Once the exception is thrown, the remaining statements in that `try` block are skipped.

## 9. Multiple catch Blocks

A `try` block can be followed by multiple `catch` blocks.

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] a = {10, 20};
            System.out.println(a[5]);
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic error");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Invalid array index");
        } catch (Exception e) {
            System.out.println("Other exception");
        }
    }
}
```

**Output:**
```text
Invalid array index
```

**Important:** More specific exception types must appear before broader types that would otherwise make them unreachable.

## 10. The finally Block

The `finally` block is used for cleanup code that should normally execute whether or not an exception occurs.

```java
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Inside try");
        } catch (Exception e) {
            System.out.println("Inside catch");
        } finally {
            System.out.println("Inside finally");
        }
    }
}
```

**Output:**
```text
Inside try
Inside finally
```

Important points:
- `finally` usually executes whether an exception occurs or not.
- It can be used for cleanup.
- It may not execute if the JVM terminates abruptly, for example through `System.exit()`.
- Try-with-resources is generally preferred for closing files and similar resources.

## 11. The throw Keyword

The `throw` keyword explicitly throws an exception object.

```java
public class Main {
    public static void main(String[] args) {
        int age = 15;

        if (age < 18) {
            throw new IllegalArgumentException(
                "Age must be at least 18"
            );
        }

        System.out.println("Eligible");
    }
}
```

If executed with `age = 15`, the program throws an `IllegalArgumentException` and does not print `Eligible`.

## 12. The throws Keyword

The `throws` keyword declares exceptions that a method may propagate to its caller.

```java
import java.io.IOException;

class Demo {
    static void readData() throws IOException {
        throw new IOException("Read failed");
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            Demo.readData();
        } catch (IOException e) {
            System.out.println("Handled: " + e.getMessage());
        }
    }
}
```

**Output:**
```text
Handled: Read failed
```

### throw vs throws

| `throw` | `throws` |
|---|---|
| Explicitly throws an exception object | Declares possible exceptions in a method signature |
| Used inside a method or block | Used in a method or constructor declaration |
| Followed by an exception object | Followed by exception type names |
| Throws one object at a time | Can declare multiple exception types |

## 13. Custom Exceptions

A custom exception is a user-defined exception class.

To create a checked custom exception, extend `Exception`.

```java
class InvalidAgeException extends Exception {
    InvalidAgeException(String message) {
        super(message);
    }
}

public class Main {
    static void checkAge(int age)
            throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException(
                "Age must be at least 18"
            );
        }

        System.out.println("Eligible");
    }

    public static void main(String[] args) {
        try {
            checkAge(16);
        } catch (InvalidAgeException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

**Output:**
```text
Age must be at least 18
```

A custom unchecked exception can instead extend `RuntimeException`.

## 14. Exception Propagation

Exception propagation occurs when an exception is not handled in the method where it is thrown and travels up the call stack.

```java
public class Main {
    static void methodA() {
        int x = 10 / 0;
    }

    static void methodB() {
        methodA();
    }

    public static void main(String[] args) {
        try {
            methodB();
        } catch (ArithmeticException e) {
            System.out.println("Exception handled in main");
        }
    }
}
```

**Output:**
```text
Exception handled in main
```

The exception propagates from `methodA()` through `methodB()` until it is caught in `main()`.

## 15. try-with-resources

Try-with-resources automatically closes resources that implement `AutoCloseable`.

It is commonly used with files, readers, writers, and database resources.

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileWriter writer =
                 new FileWriter("notes.txt")) {
            writer.write("Java exception handling");
            System.out.println("File written");
        } catch (IOException e) {
            System.out.println("I/O error");
        }
    }
}
```

The writer is automatically closed when control leaves the `try` block.

## 16. Common Exceptions

| Exception | Typical Cause |
|---|---|
| `ArithmeticException` | Integer division by zero |
| `NullPointerException` | Using a null reference where an object is required |
| `ArrayIndexOutOfBoundsException` | Invalid array index |
| `NumberFormatException` | Invalid numeric string conversion |
| `ClassCastException` | Invalid runtime reference cast |
| `IOException` | Input-output failure |
| `FileNotFoundException` | Requested file cannot be found or opened |

## 17. Important Exam Questions

1. What is a package?
2. What is the purpose of `import`?
3. What is static import?
4. Explain Java access modifiers.
5. What is an exception?
6. Differentiate between checked and unchecked exceptions.
7. Explain `try`, `catch`, and `finally`.
8. Differentiate between `throw` and `throws`.
9. What is exception propagation?
10. How do you create a custom exception?
11. What is try-with-resources?
12. Why must specific catch blocks appear before broader ones?
13. Differentiate between `Exception` and `Error`.
14. What happens when an exception is not handled?

## 18. Quick Revision

- Packages organize related types.
- `import` allows types to be referred to by simpler names.
- `java.lang` is imported automatically.
- `Throwable` is the root of Java's exception hierarchy.
- Checked exceptions must generally be caught or declared.
- Runtime exceptions are unchecked.
- `try` contains code that may throw exceptions.
- `catch` handles matching exceptions.
- `finally` is used for cleanup.
- `throw` explicitly throws an exception object.
- `throws` declares possible exceptions in a method signature.
- Custom checked exceptions can extend `Exception`.
- Try-with-resources automatically closes supported resources.
