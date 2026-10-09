# Week 3 — Input-Output Handling in Java

## 1. Introduction to Input-Output

Input-Output (I/O) operations allow a Java program to receive input and produce output.

- **Input:** Data supplied to a program.
- **Output:** Data produced by a program.

Java provides I/O classes mainly through the `java.io` package. The `java.util` package also provides the `Scanner` class for convenient input.

---

## 2. Standard Input and Output

Java provides three standard streams through the `System` class.

| Stream | Purpose |
|---|---|
| `System.in` | Standard input |
| `System.out` | Standard output |
| `System.err` | Standard error output |

### print()

Displays output without automatically adding a newline.

```java
System.out.print("Hello ");
System.out.print("Java");
```

Output:

```text
Hello Java
```

### println()

Displays output followed by a newline.

```java
System.out.println("Hello");
System.out.println("Java");
```

Output:

```text
Hello
Java
```

### printf()

Displays formatted output.

```java
int age = 20;
double marks = 85.5;

System.out.printf("Age: %d%n", age);
System.out.printf("Marks: %.1f%n", marks);
```

Output:

```text
Age: 20
Marks: 85.5
```

### Common Format Specifiers

| Specifier | Meaning |
|---|---|
| `%d` | Integer |
| `%f` | Floating-point value |
| `%.2f` | Floating-point value with 2 digits after decimal |
| `%c` | Character |
| `%s` | String |
| `%n` | Platform-specific newline |

---

## 3. Taking Input Using Scanner

The `Scanner` class belongs to the `java.util` package.

It allows a program to read different types of input.

```java
import java.util.Scanner;

class InputDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter your age: ");
        int age = sc.nextInt();

        System.out.println("Age: " + age);

        sc.close();
    }
}
```

### Common Scanner Methods

| Method | Purpose |
|---|---|
| `nextInt()` | Reads an integer |
| `nextLong()` | Reads a long integer |
| `nextFloat()` | Reads a float |
| `nextDouble()` | Reads a double |
| `next()` | Reads one token |
| `nextLine()` | Reads the remaining line |
| `nextBoolean()` | Reads a boolean token |

### Difference Between next() and nextLine()

- `next()` reads one token and stops at whitespace.
- `nextLine()` reads the remaining text on the current line.

Example input:

```text
Hello Java Programming
```

```java
String word = sc.next();
```

Result:

```text
Hello
```

Whereas:

```java
String line = sc.nextLine();
```

can read the whole line, including spaces.

### Important Trap: Mixing nextInt() and nextLine()

Consider:

```java
int age = sc.nextInt();
String name = sc.nextLine();
```

After `nextInt()` reads the number, the newline may remain in the input. The following `nextLine()` can consume that remaining newline and return an empty string.

A common solution is:

```java
int age = sc.nextInt();
sc.nextLine(); // Consume the remaining newline
String name = sc.nextLine();
```

---

## 4. System.in and Reading a Character

`System.in` is an `InputStream` used for standard input.

The `read()` method reads a byte and returns its value as an integer. It returns `-1` when the stream reaches its end.

```java
import java.io.IOException;

class CharacterInput {
    public static void main(String[] args) throws IOException {
        System.out.print("Enter a character: ");

        int value = System.in.read();

        System.out.println((char) value);
    }
}
```

For simple ASCII input, casting the returned value to `char` displays the corresponding character.

**Remember:** `System.in.read()` returns an `int`, not a `char`.

---

## 5. Byte Streams and Character Streams

Java I/O streams are divided into two major categories.

### Byte Streams

Byte streams process raw bytes. They are commonly used for binary data, such as images, audio and PDFs.

Main abstract classes:

- `InputStream`
- `OutputStream`

Examples:

- `FileInputStream`
- `FileOutputStream`
- `BufferedInputStream`
- `BufferedOutputStream`

### Character Streams

Character streams are designed for text and character data.

Main abstract classes:

- `Reader`
- `Writer`

Examples:

- `FileReader`
- `FileWriter`
- `BufferedReader`
- `BufferedWriter`

### Comparison

| Byte Streams | Character Streams |
|---|---|
| Process bytes | Process characters |
| Based on `InputStream` and `OutputStream` | Based on `Reader` and `Writer` |
| Commonly used for binary data | Commonly used for text |
| Example: `FileInputStream` | Example: `FileReader` |

Character streams handle character encoding. For explicit control over an encoding, Java provides classes such as `InputStreamReader` and `OutputStreamWriter`.

---

## 6. BufferedReader

`BufferedReader` reads text efficiently by buffering characters.

It belongs to the `java.io` package.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class BufferedInputDemo {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(
            new InputStreamReader(System.in)
        );

        System.out.print("Enter your name: ");
        String name = br.readLine();

        System.out.println("Hello, " + name);
    }
}
```

### Important Methods

- `read()` — Reads a character and returns its integer value.
- `readLine()` — Reads a line of text.
- `close()` — Closes the reader.

`readLine()` returns `null` when the end of the stream is reached before another line is available.

### Scanner vs BufferedReader

| Scanner | BufferedReader |
|---|---|
| Convenient for reading different data types | Reads text efficiently |
| Provides methods such as `nextInt()` | Provides `readLine()` and `read()` |
| Can parse tokens using delimiters | Usually requires manual parsing |
| Belongs to `java.util` | Belongs to `java.io` |

For example, with `BufferedReader`, an integer can be read using:

```java
int number = Integer.parseInt(br.readLine());
```

---

## 7. File Handling

Java provides classes for reading data from and writing data to files.

### Writing to a File

```java
import java.io.FileWriter;
import java.io.IOException;

class FileWriteDemo {
    public static void main(String[] args) throws IOException {
        try (FileWriter writer = new FileWriter("output.txt")) {
            writer.write("Learning Java I/O");
        }
    }
}
```

By default, `FileWriter` creates or truncates the destination file.

To append instead:

```java
FileWriter writer = new FileWriter("output.txt", true);
```

The second argument `true` enables append mode.

### Reading from a File

```java
import java.io.FileReader;
import java.io.IOException;

class FileReadDemo {
    public static void main(String[] args) throws IOException {
        try (FileReader reader = new FileReader("output.txt")) {
            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }
}
```

The `read()` method returns `-1` when the end of the stream is reached.

### Important File Classes

| Class | Purpose |
|---|---|
| `File` | Represents a file or directory path |
| `FileInputStream` | Reads bytes from a file |
| `FileOutputStream` | Writes bytes to a file |
| `FileReader` | Reads text characters |
| `FileWriter` | Writes text characters |
| `BufferedReader` | Reads buffered text |
| `BufferedWriter` | Writes buffered text |

---

## 8. The File Class

The `File` class belongs to `java.io`.

It represents a file or directory path and provides methods to examine paths and perform certain file operations.

```java
import java.io.File;

class FileInfoDemo {
    public static void main(String[] args) {
        File file = new File("output.txt");

        System.out.println("Exists: " + file.exists());
        System.out.println("Name: " + file.getName());
        System.out.println("Path: " + file.getPath());
        System.out.println("Is file: " + file.isFile());
    }
}
```

### Important Methods

- `exists()` — Checks whether the path exists.
- `getName()` — Returns the name of the file or directory.
- `getPath()` — Returns the path.
- `length()` — Returns the file length in bytes for a regular file.
- `isFile()` — Checks whether the path refers to a regular file.
- `isDirectory()` — Checks whether the path refers to a directory.
- `mkdir()` — Attempts to create a directory.
- `delete()` — Attempts to delete a file or empty directory.

**Important:** Creating a `File` object does not automatically create the actual file.

---

## 9. Exception Handling in I/O

I/O operations may fail because a file is missing, permissions are insufficient, or another I/O problem occurs.

Many I/O operations can throw `IOException`.

### Using try-catch

```java
import java.io.FileReader;
import java.io.IOException;

class ExceptionDemo {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("data.txt")) {
            System.out.println("File opened successfully");
        } catch (IOException e) {
            System.out.println("I/O error: " + e.getMessage());
        }
    }
}
```

### Try-with-resources

Try-with-resources automatically closes resources that implement `AutoCloseable`.

```java
try (FileReader reader = new FileReader("data.txt")) {
    // Read from the file
}
```

This helps prevent resource leaks.

---

## 10. Serialization

Serialization converts an object's state into a byte stream so that it can be stored or transmitted.

Deserialization reconstructs an object from the serialized representation.

Java provides:

- `ObjectOutputStream`
- `ObjectInputStream`
- `Serializable`

Example:

```java
import java.io.Serializable;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;

    int id;
    String name;
}
```

An object can be written using `ObjectOutputStream` and read using `ObjectInputStream`.

**Security note:** Never deserialize untrusted data using Java's native object deserialization without appropriate safeguards.

---

## 11. Important Exam Points

- `System.in` is the standard input stream.
- `System.out` is the standard output stream.
- `System.err` is the standard error stream.
- `print()` does not automatically add a newline.
- `println()` adds a newline.
- `Scanner` belongs to `java.util`.
- `BufferedReader` belongs to `java.io`.
- `InputStream` and `OutputStream` are byte-stream base classes.
- `Reader` and `Writer` are character-stream base classes.
- `System.in.read()` returns an integer.
- Stream-reading methods may return `-1` at end-of-stream.
- `BufferedReader.readLine()` returns `null` at end-of-stream.
- Many I/O operations can throw `IOException`.
- Try-with-resources closes resources automatically.
- Creating a `File` object does not itself create a file.
- Serialization stores an object's state in a byte stream.

---

## 12. Quick Revision Table

| Task | Common class or method |
|---|---|
| Read an integer | `Scanner.nextInt()` |
| Read one token | `Scanner.next()` |
| Read a line | `Scanner.nextLine()` |
| Read a line efficiently | `BufferedReader.readLine()` |
| Print a line | `System.out.println()` |
| Read file bytes | `FileInputStream` |
| Write file bytes | `FileOutputStream` |
| Read text | `FileReader` |
| Write text | `FileWriter` |
| Inspect a path | `File` |
| Handle I/O errors | `IOException` |
| Serialize an object | `ObjectOutputStream` |

---

## Key Takeaways

- Java provides several ways to perform input and output.
- `Scanner` is convenient for basic input.
- Buffered readers are useful for efficient text reading.
- Byte streams handle bytes, while character streams are designed for text.
- File operations should handle possible I/O exceptions.
- Try-with-resources helps close resources safely.
