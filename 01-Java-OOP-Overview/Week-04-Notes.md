# Week 4: Encapsulation in Java

## 1. What Is Encapsulation?

Encapsulation is one of the four fundamental pillars of Object-Oriented Programming (OOP).

**Definition:** Encapsulation is the process of combining data (variables) and the methods that operate on that data into a single unit called a class, while controlling access to the data.

It is commonly implemented by:
- Declaring instance variables as `private`.
- Providing public getter and setter methods to access or modify them when needed.

### Advantages of Encapsulation

1. Data hiding and improved security.
2. Better control over how data is accessed and modified.
3. Easier maintenance and modification of code.
4. Improved modularity.
5. Ability to validate data before changing an object's state.

## 2. Example of Encapsulation

```java
class Student {
    private String name;
    private int age;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        }
    }

    public int getAge() {
        return age;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();

        s.setName("Arpan");
        s.setAge(20);

        System.out.println(s.getName());
        System.out.println(s.getAge());
    }
}
```

**Output:**
```text
Arpan
20
```

**Explanation:**
- `private` prevents direct access to the fields from outside the `Student` class.
- `setName()` and `setAge()` modify the fields.
- `getName()` and `getAge()` return their values.
- `this.name` refers to the current object's `name` field.
- `setAge()` validates the input before assigning it.

## 3. Data Hiding

Data hiding means restricting direct access to the internal data of a class.

In Java, the `private` access modifier is commonly used to achieve data hiding.

Example:

```java
class Account {
    private double balance;

    public double getBalance() {
        return balance;
    }
}
```

The following statement is invalid outside the `Account` class:

```java
// Account a = new Account();
// a.balance = 5000; // Compilation error
```

The field can be accessed internally through methods provided by the class.

**Important:** Encapsulation and data hiding are related, but they are not exactly the same. Encapsulation bundles data and methods together; data hiding restricts access to implementation details.

## 4. Access Modifiers in Java

Access modifiers control where classes, methods, constructors, and fields can be accessed.

| Modifier | Same Class | Same Package | Subclass in Another Package | Other Classes |
|---|---|---|---|---|
| `private` | Yes | No | No | No |
| Default (package-private) | Yes | Yes | No, not through ordinary package access | No |
| `protected` | Yes | Yes | Yes, subject to Java's subclass access rules | No |
| `public` | Yes | Yes | Yes | Yes |

### 4.1 private

Accessible only within the declaring top-level class or enclosing class context permitted by Java's access rules.

```java
class Demo {
    private int x = 10;

    public void display() {
        System.out.println(x);
    }
}
```

### 4.2 Default Access

When no access modifier is specified, the member has package-private access.

```java
class Demo {
    int x = 10;
}
```

It is accessible to classes in the same package, but not ordinarily to classes in another package.

### 4.3 protected

Accessible within the same package and, under Java's subclass access rules, in subclasses in other packages.

```java
class Parent {
    protected int x = 10;
}
```

### 4.4 public

Accessible wherever the declaring type and member are otherwise accessible.

```java
class Demo {
    public int x = 10;
}
```

**Exam point:** A top-level class can normally be declared `public` or have default access. It cannot be declared `private` or `protected`.

## 5. Getters and Setters

A **getter** returns a field's value.

A **setter** changes a field's value.

Example:

```java
class Employee {
    private int salary;

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        if (salary >= 0) {
            this.salary = salary;
        }
    }
}
```

### Naming Convention

- Getter for a field named `age`: `getAge()`
- Setter for a field named `age`: `setAge(int age)`
- Boolean getter: often `isActive()` or `getActive()`

### Why Use Setters?

A setter can validate input before changing the field.

For example, it can prevent a negative salary from being assigned.

**Important:** Getters and setters are not compulsory for every field. A class can intentionally expose read-only data or provide methods that perform specific operations instead.

## 6. The this Keyword

The `this` keyword refers to the current object.

### 6.1 Distinguishing Instance Variables from Parameters

```java
class Student {
    private String name;

    public void setName(String name) {
        this.name = name;
    }
}
```

Here:
- `this.name` refers to the instance variable.
- `name` refers to the method parameter.

Without `this`, the assignment `name = name;` would assign the parameter to itself and leave the instance field unchanged.

### 6.2 Calling Another Constructor

`this()` can call another constructor in the same class.

```java
class Student {
    String name;
    int age;

    Student() {
        this("Unknown", 0);
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

**Exam point:** A constructor call using `this()` must be the first statement in the constructor body.

## 7. Constructors and Encapsulation

A constructor initializes a newly created object.

```java
class Student {
    private String name;
    private int age;

    Student(String name, int age) {
        this.name = name;

        if (age >= 0) {
            this.age = age;
        }
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

A constructor can initialize private fields directly. A setter is not required for every initialization.

### Constructor vs Setter

| Constructor | Setter |
|---|---|
| Runs during object creation | Called explicitly after an object exists |
| Initializes initial state | Modifies existing state |
| Has the same name as the class | Has an ordinary method name |
| Has no return type | Has a declared return type, often `void` |

## 8. Read-Only and Write-Only Access

### Read-Only Field

Provide a getter but no public setter.

```java
class Product {
    private final int id;

    Product(int id) {
        this.id = id;
    }

    public int getId() {
        return id;
    }
}
```

The `final` field is assigned during initialization and cannot subsequently be reassigned.

### Write-Only-Style Field

A class can expose a setter but no getter, although this design is less common and depends on the use case.

```java
class PasswordStore {
    private String password;

    public void setPassword(String password) {
        this.password = password;
    }
}
```

This example demonstrates access design only; storing real passwords requires secure password hashing and other protections.

## 9. Encapsulation vs Abstraction

These concepts are related but distinct.

| Encapsulation | Abstraction |
|---|---|
| Bundles data and methods in a class | Exposes essential features while hiding unnecessary implementation details |
| Controls access to internal state | Focuses on what an object does rather than how it does it |
| Commonly uses access modifiers and controlled methods | Commonly uses interfaces and abstract classes |
| Example: private balance with deposit and withdraw methods | Example: a `Payment` interface with a `pay()` method |

## 10. Complete Example: Bank Account

```java
class BankAccount {
    private String accountHolder;
    private double balance;

    public BankAccount(String accountHolder, double initialBalance) {
        this.accountHolder = accountHolder;

        if (initialBalance >= 0) {
            this.balance = initialBalance;
        }
    }

    public String getAccountHolder() {
        return accountHolder;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }

        return false;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("Arpan", 1000);

        account.deposit(500);
        account.withdraw(200);

        System.out.println(account.getAccountHolder());
        System.out.println(account.getBalance());
    }
}
```

**Output:**
```text
Arpan
1300.0
```

**Why this is encapsulation:**
- `balance` is private.
- External code cannot directly assign an arbitrary balance.
- Deposits and withdrawals are performed through controlled methods.
- The methods enforce basic validation rules.

## 11. Common Exam Questions

1. Define encapsulation with an example.
2. What is data hiding?
3. Differentiate between encapsulation and abstraction.
4. Explain the four access modifiers in Java.
5. What is the purpose of getters and setters?
6. What is the difference between `private` and `protected`?
7. Explain the use of the `this` keyword.
8. What is the difference between a constructor and a setter?
9. Can a private variable be accessed directly outside its class?
10. Why is encapsulation useful in object-oriented programming?

## 12. Quick Revision

- Encapsulation bundles data and related methods into a class.
- Data hiding restricts direct access to internal state.
- `private` is commonly used for encapsulated fields.
- Getters read values; setters modify values.
- Setters can validate data.
- `this` refers to the current object.
- `this()` calls another constructor in the same class.
- `protected` allows same-package access and qualified subclass access across packages.
- Encapsulation and abstraction solve related but different problems.
- Encapsulation improves maintainability and control over object state.
