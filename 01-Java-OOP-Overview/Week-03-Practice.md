# Week 3: Java Input-Output Practice

This file contains practical Java programs covering standard input/output, Scanner, BufferedReader, file handling, and exception handling.

## 1. Read and Print User Input Using Scanner

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter your name: ");
        String name = sc.nextLine();

        System.out.print("Enter your age: ");
        int age = sc.nextInt();

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);

        sc.close();
    }
}
```

**Concepts:** `Scanner`, `nextLine()`, `nextInt()`, `System.in`, `System.out`.

## 2. Demonstrate the nextInt() and nextLine() Trap

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter your age: ");
        int age = sc.nextInt();
        sc.nextLine(); // Consume the leftover newline

        System.out.print("Enter your full name: ");
        String name = sc.nextLine();

        System.out.println(name + " is " + age + " years old.");

        sc.close();
    }
}
```

**Exam point:** `nextInt()` does not consume the newline after the number. The extra `nextLine()` consumes it before reading the full name.

## 3. Display Formatted Output Using printf()

```java
public class Main {
    public static void main(String[] args) {
        String name = "Arpan";
        int age = 20;
        double marks = 85.75;

        System.out.printf("Name: %s%n", name);
        System.out.printf("Age: %d%n", age);
        System.out.printf("Marks: %.2f%n", marks);
    }
}
```

**Concepts:**
- `%s` — String
- `%d` — integer
- `%f` — floating-point number
- `%.2f` — floating-point number rounded to two decimal places
- `%n` — platform-independent line separator

## 4. Read a Character Using System.in.read()

```java
import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        System.out.print("Enter a character: ");

        int ch = System.in.read();

        System.out.println("Character entered: " + (char) ch);
    }
}
```

**Exam point:** `System.in.read()` returns an `int`, representing the byte read or `-1` when the stream reaches its end.

## 5. Read Input Using BufferedReader

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(
            new InputStreamReader(System.in)
        );

        System.out.print("Enter your name: ");
        String name = br.readLine();

        System.out.print("Enter your age: ");
        int age = Integer.parseInt(br.readLine());

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
    }
}
```

**Exam point:** `BufferedReader.readLine()` returns a String. Use methods such as `Integer.parseInt()` to convert text into numeric values.

## 6. Write Data to a File

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("notes.txt")) {
            writer.write("Java Input-Output\n");
            writer.write("File handling example");
            System.out.println("Data written successfully.");
        } catch (IOException e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }
}
```

**Exam point:** `FileWriter` writes character data. By default, this constructor overwrites an existing file's contents.

## 7. Append Data to an Existing File

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("notes.txt", true)) {
            writer.write("\nThis line is appended.");
            System.out.println("Data appended successfully.");
        } catch (IOException e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }
}
```

**Exam point:** The second argument `true` enables append mode.

## 8. Read Data from a File

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("notes.txt")) {
            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        } catch (IOException e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }
}
```

**Exam point:** `read()` returns the character value as an `int`, or `-1` at the end of the stream.

## 9. Check Whether a File Exists

```java
import java.io.File;

public class Main {
    public static void main(String[] args) {
        File file = new File("notes.txt");

        if (file.exists()) {
            System.out.println("File exists.");
            System.out.println("File size: " + file.length() + " bytes");
        } else {
            System.out.println("File does not exist.");
        }
    }
}
```

**Exam point:** Creating a `File` object does not itself create a physical file.

## 10. Handle an Input-Output Exception

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("missing.txt")) {
            System.out.println(reader.read());
        } catch (IOException e) {
            System.out.println("Unable to read the file.");
        }
    }
}
```

**Concepts:** `try`, `catch`, `IOException`, and try-with-resources.

Try-with-resources automatically closes resources that implement `AutoCloseable`.

## 11. Important Differences

| Feature | Scanner | BufferedReader |
|---|---|---|
| Package | `java.util` | `java.io` |
| Reads tokens | Yes | No, reads text lines |
| Reads a full line | `nextLine()` | `readLine()` |
| Converts numbers | Built-in methods such as `nextInt()` | Parse methods such as `Integer.parseInt()` |
| Typical use | Convenient user input | Efficient text input |

| Byte Streams | Character Streams |
|---|---|
| `InputStream` / `OutputStream` | `Reader` / `Writer` |
| Handle bytes | Handle character data |
| Useful for binary data | Useful for text data |

## 12. Practice Questions

Try answering these without looking at the notes.

1. What is the difference between `print()` and `println()`?
2. What is the difference between `next()` and `nextLine()`?
3. Why does `System.in.read()` return an `int`?
4. What is the difference between `FileReader` and `FileInputStream`?
5. How can `FileWriter` append data instead of overwriting a file?
6. What is try-with-resources, and why is it useful?
7. What does `read()` return when the end of a stream is reached?
8. What is the difference between `Scanner` and `BufferedReader`?
9. Does `new File("notes.txt")` create a physical file?
10. What is the purpose of `IOException`?

## Quick Revision

- `System.in` — standard input.
- `System.out` — standard output.
- `System.err` — standard error output.
- `Scanner` — convenient input parsing.
- `BufferedReader` — buffered text input.
- `FileReader` / `FileWriter` — character-based file I/O.
- `FileInputStream` / `FileOutputStream` — byte-based file I/O.
- `IOException` — checked exception for many I/O failures.
- Try-with-resources — automatically closes supported resources.
