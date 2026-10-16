# Week 8 — Java Packages & Exception Handling Practice

## Part A: Predict the Output

### Q1. Order of catch blocks

```java
public class Main {
    public static void main(String[] args) {
        try {
            int x = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic");
        } catch (Exception e) {
            System.out.println("Exception");
        }
    }
}
```

**Answer:**
```text
Arithmetic
```

**Why?** The first compatible catch block executes. Specific exception classes should appear before their more general parent classes.

---

### Q2. Unreachable catch block

```java
try {
    System.out.println("Hello");
} catch (Exception e) {
    System.out.println("Exception");
} catch (ArithmeticException e) {
    System.out.println("Arithmetic");
}
```

**Answer:** Compilation error.

**Why?** `ArithmeticException` is a subclass of `Exception`. The earlier catch already catches it, making the later catch unreachable.

---

### Q3. `finally` execution

```java
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Try");
        } catch (Exception e) {
            System.out.println("Catch");
        } finally {
            System.out.println("Finally");
        }
    }
}
```

**Answer:**
```text
Try
Finally
```

**Why?** The `finally` block normally executes whether or not an exception occurs.

---

### Q4. Return and `finally`

```java
public class Main {
    static int test() {
        try {
            return 10;
        } finally {
            System.out.println("Finally");
        }
    }

    public static void main(String[] args) {
        System.out.println(test());
    }
}
```

**Answer:**
```text
Finally
10
```

**Why?** The `finally` block runs before the method completes its return.

---

### Q5. Exception propagation

```java
public class Main {
    static void first() {
        second();
    }

    static void second() {
        int x = 5 / 0;
    }

    public static void main(String[] args) {
        try {
            first();
        } catch (ArithmeticException e) {
            System.out.println("Handled");
        }
    }
}
```

**Answer:**
```text
Handled
```

**Why?** The exception propagates up the call stack until a matching handler catches it.

---

### Q6. `throw` versus `throws`

```java
public class Main {
    static void check(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Underage");
        }
    }

    public static void main(String[] args) {
        try {
            check(16);
        } catch (IllegalArgumentException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

**Answer:**
```text
Underage
```

**Why?** `throw` explicitly throws an exception object. `getMessage()` returns the message supplied when the exception was created.

---

### Q7. Checked exception

```java
import java.io.FileReader;

public class Main {
    public static void main(String[] args) {
        FileReader reader = new FileReader("data.txt");
    }
}
```

**Answer:** Compilation error.

**Why?** `FileReader` can throw the checked exception `FileNotFoundException`. It must be caught or declared using `throws`.

---

### Q8. Catching a parent exception

```java
try {
    int[] arr = new int[2];
    System.out.println(arr[5]);
} catch (Exception e) {
    System.out.println("Error");
}
```

**Answer:**
```text
Error
```

**Why?** `ArrayIndexOutOfBoundsException` is a subclass of `RuntimeException`, which is a subclass of `Exception`.

---

## Part B: Write the Programs

### Q9. Handle division by zero

Write a program that accepts two integers and divides the first by the second. Handle division by zero using `ArithmeticException`.

**Solution:**

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int a = sc.nextInt();
        int b = sc.nextInt();

        try {
            System.out.println(a / b);
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero");
        }

        sc.close();
    }
}
```

### Q10. Use multiple catch blocks

Write a program that handles both division by zero and invalid array indexing.

**Solution:**

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] a = {10, 20, 30};
            int x = 10 / 0;
            System.out.println(a[5]);
        } catch (ArithmeticException e) {
            System.out.println("Division error");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Index error");
        }
    }
}
```

**Important:** Only the first exception thrown is handled during that execution. Here, the array access is never reached.

### Q11. Create a custom exception

Create an exception called `InvalidAgeException`. Throw it if the supplied age is below 18.

**Solution:**

```java
class InvalidAgeException extends Exception {
    InvalidAgeException(String message) {
        super(message);
    }
}

public class Main {
    static void checkAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or above");
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
Age must be 18 or above
```

**Remember:**
- `extends Exception` creates a checked custom exception.
- `throw` throws the exception.
- `throws` declares a possible exception.

### Q12. Use try-with-resources

Write a program that reads a text file using `FileReader` and automatically closes it.

**Solution:**

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("data.txt")) {
            int ch;
            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        } catch (IOException e) {
            System.out.println("File error");
        }
    }
}
```

**Why use it?** Resources implementing `AutoCloseable` are closed automatically when the try-with-resources statement finishes.

---

## Part C: Packages and Access Modifiers

### Q13. Package declaration

Which statement must appear before ordinary imports and class declarations when declaring a named package?

**Answer:** The `package` declaration.

Example:

```java
package mypack;

import java.util.Scanner;

public class Demo {
}
```

A package declaration, when present, must be the first non-comment statement in the compilation unit.

### Q14. Importing a class

Suppose `Student` is a public class in package `college`.

Write the import statement needed to use it in another package.

**Answer:**

```java
import college.Student;
```

### Q15. Access modifiers

Complete this table.

| Modifier | Same class | Same package | Subclass in another package | Unrelated class in another package |
|---|---|---|---|---|
| `private` | Yes | No | No | No |
| default (package-private) | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes, subject to subclass access rules | No |
| `public` | Yes | Yes | Yes | Yes |

**Exam trap:** `protected` access across packages is not unrestricted. A subclass can access the inherited protected member under Java's subclass access rules.

---

## Part D: Quick NPTEL Revision

Answer these without looking at the solutions.

1. Is `NullPointerException` checked or unchecked?
2. Is `IOException` checked or unchecked?
3. Which superclass is the parent of both `Exception` and `Error`?
4. Can a `try` block exist without `catch` if it has a `finally` block?
5. Can one `try` block have multiple `catch` blocks?
6. Does `finally` always execute under every possible circumstance?
7. What is the difference between `throw` and `throws`?
8. Can a checked exception be ignored without handling or declaring it?
9. What happens if a superclass catch block appears before a subclass catch block?
10. Does `import java.util.*;` import classes from `java.util.concurrent` automatically?

### Answers

1. Unchecked.
2. Checked.
3. `Throwable`.
4. Yes.
5. Yes.
6. No. For example, abrupt JVM termination can prevent it from executing.
7. `throw` throws an exception object; `throws` declares exceptions a method may propagate.
8. No, not when the exception is subject to Java's checked-exception rules.
9. The later subclass catch becomes unreachable and causes a compilation error.
10. No. Wildcard imports do not automatically include subpackages.

---

## Part E: Common Exam Traps

- `ArithmeticException`, `NullPointerException`, and `ArrayIndexOutOfBoundsException` are unchecked exceptions.
- `IOException` and `SQLException` are checked exceptions.
- `Error` and `Exception` both extend `Throwable`.
- A `finally` block usually executes even if `try` or `catch` returns, but it is not guaranteed under all circumstances.
- If `finally` itself returns or throws, it can override an earlier return or exception. Avoid this pattern in real code.
- Catch subclasses before superclasses.
- A method may declare multiple exception types using `throws`.
- A custom checked exception can extend `Exception`; an unchecked custom exception can extend `RuntimeException`.
- Package names are conventionally lowercase.
- `java.lang` is imported automatically; other packages generally need explicit imports or qualified names.
- `import package.*;` imports accessible types in that package, not its subpackages.

## Completion Checklist

- [ ] I can distinguish checked and unchecked exceptions.
- [ ] I can predict output involving `try`, `catch`, and `finally`.
- [ ] I understand exception propagation.
- [ ] I can explain `throw` versus `throws`.
- [ ] I can create a custom exception.
- [ ] I understand package declarations, imports, and access modifiers.
- [ ] I can identify unreachable catch blocks.
