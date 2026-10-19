# Week 4: Encapsulation Practice

## 1. Basic Encapsulation

Create a `Student` class with private fields `name` and `age`. Add public getters and setters.

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

**Expected output:**
```text
Arpan
20
```

## 2. Demonstrate Data Hiding

```java
class Employee {
    private double salary = 25000;

    public double getSalary() {
        return salary;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee e = new Employee();

        System.out.println(e.getSalary());

        // System.out.println(e.salary);
        // Compilation error: salary is private.
    }
}
```

**Expected output:**
```text
25000.0
```

**Question:** Why can't `main()` directly access `salary`?

## 3. Validate Data Using a Setter

```java
class Product {
    private double price;

    public void setPrice(double price) {
        if (price >= 0) {
            this.price = price;
        } else {
            System.out.println("Invalid price");
        }
    }

    public double getPrice() {
        return price;
    }
}

public class Main {
    public static void main(String[] args) {
        Product p = new Product();

        p.setPrice(500);
        System.out.println(p.getPrice());

        p.setPrice(-100);
        System.out.println(p.getPrice());
    }
}
```

**Expected output:**
```text
500.0
Invalid price
500.0
```

**Concept:** A setter can validate data before modifying a field.

## 4. Understand the this Keyword

```java
class Person {
    private String name;

    Person(String name) {
        this.name = name;
    }

    public void display() {
        System.out.println("Name: " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Arpan");
        p.display();
    }
}
```

**Expected output:**
```text
Name: Arpan
```

**Question:** What would happen if the constructor used `name = name;` instead of `this.name = name;`?

## 5. Constructor and Encapsulation

```java
class BankAccount {
    private double balance;

    BankAccount(double balance) {
        if (balance >= 0) {
            this.balance = balance;
        }
    }

    public double getBalance() {
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount a = new BankAccount(5000);
        BankAccount b = new BankAccount(-200);

        System.out.println(a.getBalance());
        System.out.println(b.getBalance());
    }
}
```

**Expected output:**
```text
5000.0
0.0
```

## 6. Read-Only Field

```java
class Student {
    private final int rollNumber;

    Student(int rollNumber) {
        this.rollNumber = rollNumber;
    }

    public int getRollNumber() {
        return rollNumber;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student(101);
        System.out.println(s.getRollNumber());
    }
}
```

**Expected output:**
```text
101
```

**Question:** Why is there no setter for `rollNumber`?

## 7. Bank Account with Deposit and Withdrawal

```java
class BankAccount {
    private double balance;

    BankAccount(double balance) {
        if (balance >= 0) {
            this.balance = balance;
        }
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

    public double getBalance() {
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);

        account.deposit(500);
        account.withdraw(200);

        System.out.println(account.getBalance());
    }
}
```

**Expected output:**
```text
1300.0
```

## 8. Practice Questions

Try to answer these without looking at the notes.

1. What is encapsulation?
2. What is data hiding?
3. Why are fields commonly declared `private`?
4. What is the difference between a getter and a setter?
5. What does `this` refer to?
6. What is the difference between `private` and `public`?
7. Why might a class provide a getter but no setter?
8. Can a constructor initialize a private field?
9. How does encapsulation help validate data?
10. Differentiate between encapsulation and abstraction.

## 9. Challenge Questions

Complete these independently:

- Create a `Book` class with private `title`, `author`, and `price` fields.
- Prevent the price from becoming negative.
- Add getters and suitable setters.
- Create three `Book` objects and display their details.
- Explain how your class demonstrates encapsulation.

## Key Takeaways

- Keep internal fields private when direct access is unnecessary.
- Expose controlled methods for operations on object state.
- Use constructors to initialize objects.
- Use `this` to distinguish instance fields from parameters.
- Validate values before updating an object's state.
