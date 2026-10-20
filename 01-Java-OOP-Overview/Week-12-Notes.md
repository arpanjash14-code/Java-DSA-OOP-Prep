# Week 12 — JDBC & Database Connectivity

## 1. What Is JDBC?

**JDBC (Java Database Connectivity)** is a Java API that allows Java applications to connect to databases, execute SQL statements, and process results.

It is commonly used to interact with databases such as MySQL, PostgreSQL, and Oracle.

The main JDBC package is:

```java
java.sql
```

Some additional JDBC interfaces and classes are available in `javax.sql`.

### Why is JDBC needed?

JDBC allows a Java program to:

- Establish a database connection.
- Execute SQL queries and updates.
- Retrieve records from a database.
- Insert, update, and delete data.
- Manage database transactions.

## 2. JDBC Architecture

The basic flow is:

```text
Java Application
       |
       v
    JDBC API
       |
       v
 JDBC Driver
       |
       v
    Database
```

**JDBC API:** Provides interfaces and classes for database access.

**JDBC Driver:** Enables communication between Java and a particular database.

**Database:** Stores and manages the data.

## 3. JDBC Drivers

Traditionally, JDBC drivers are classified into four types.

| Type | Name | Description |
|---|---|---|
| Type 1 | JDBC-ODBC Bridge | Translates JDBC calls into ODBC calls; obsolete |
| Type 2 | Native-API Driver | Uses native database libraries |
| Type 3 | Network Protocol Driver | Uses middleware to communicate with the database |
| Type 4 | Thin Driver | Communicates directly using the database protocol |

**Exam point:** Type 4 drivers are pure Java drivers and are widely used today.

For example, the MySQL Connector/J driver is a Type 4 JDBC driver.

## 4. Important JDBC Interfaces and Classes

| Interface/Class | Purpose |
|---|---|
| `DriverManager` | Obtains database connections |
| `Connection` | Represents a database connection |
| `Statement` | Executes basic SQL statements |
| `PreparedStatement` | Executes parameterized SQL statements |
| `CallableStatement` | Calls stored procedures |
| `ResultSet` | Holds query results |
| `SQLException` | Represents database-access errors |

Remember:

- `Statement` executes SQL statements.
- `PreparedStatement` supports parameters using `?` placeholders.
- `ResultSet` lets you read rows returned by a query.

## 5. Steps to Connect Java to a Database

The traditional JDBC workflow is:

1. Import JDBC classes.
2. Obtain a connection.
3. Create a statement.
4. Execute an SQL query or update.
5. Process the results.
6. Close resources.

Modern JDBC drivers are normally discovered automatically when their driver JAR is available. Explicitly calling `Class.forName()` is generally unnecessary with modern drivers.

## 6. Establishing a Connection

Example using MySQL:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JdbcConnectionDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/college";
        String username = "root";
        String password = "your_password";

        try (Connection con =
                 DriverManager.getConnection(url, username, password)) {
            System.out.println("Database connected successfully");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Explanation:

- `jdbc:mysql://` identifies the MySQL JDBC URL scheme.
- `localhost` is the database host.
- `3306` is the default MySQL port.
- `college` is the database name.
- `DriverManager.getConnection()` obtains a connection.

**Important:** The MySQL JDBC driver must be present in the application's dependencies, and the database must be accessible.

## 7. The Statement Interface

A `Statement` is used to execute SQL statements.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class StatementDemo {
    public static void main(String[] args) throws Exception {
        String url = "jdbc:mysql://localhost:3306/college";

        try (Connection con =
                 DriverManager.getConnection(url, "root", "your_password");
             Statement st = con.createStatement()) {

            int rows = st.executeUpdate(
                "INSERT INTO students (id, name) VALUES (1, 'Amit')"
            );

            System.out.println(rows);
        }
    }
}
```

If one row is inserted successfully, the program normally prints:

```text
1
```

`executeUpdate()` returns the number of affected rows for typical INSERT, UPDATE, and DELETE statements.

## 8. The PreparedStatement Interface

`PreparedStatement` is used for parameterized SQL queries.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

public class PreparedStatementDemo {
    public static void main(String[] args) throws Exception {
        String url = "jdbc:mysql://localhost:3306/college";

        String sql = "INSERT INTO students (id, name) VALUES (?, ?)";

        try (Connection con =
                 DriverManager.getConnection(url, "root", "your_password");
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setInt(1, 2);
            ps.setString(2, "Riya");

            int rows = ps.executeUpdate();
            System.out.println(rows);
        }
    }
}
```

### Advantages of PreparedStatement

- Supports parameterized SQL.
- Helps prevent SQL injection when values are bound correctly.
- Makes repeated execution with different parameter values convenient.
- Provides type-specific setter methods such as `setInt()` and `setString()`.

**Exam trap:** Parameter indexes start at **1**, not 0.

## 9. The ResultSet Interface

A `ResultSet` represents the rows returned by a query.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class ResultSetDemo {
    public static void main(String[] args) throws Exception {
        String url = "jdbc:mysql://localhost:3306/college";

        try (Connection con =
                 DriverManager.getConnection(url, "root", "your_password");
             Statement st = con.createStatement();
             ResultSet rs = st.executeQuery(
                 "SELECT id, name FROM students"
             )) {

            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");

                System.out.println(id + " " + name);
            }
        }
    }
}
```

### Important methods

| Method | Purpose |
|---|---|
| `next()` | Moves to the next row |
| `getInt()` | Reads an integer column |
| `getString()` | Reads a string column |
| `getDouble()` | Reads a numeric value as a double |
| `getBoolean()` | Reads a Boolean value |

**Remember:** A newly created `ResultSet` cursor is initially positioned before the first row. Call `next()` before reading the first row.

## 10. Statement vs PreparedStatement

| Statement | PreparedStatement |
|---|---|
| Executes a SQL string | Uses a parameterized SQL statement |
| Values may be concatenated into SQL | Values can be bound using setter methods |
| SQL injection risk can arise from unsafe concatenation | Helps prevent injection when used correctly |
| Suitable for simple, fixed SQL | Useful for parameterized or repeated queries |

## 11. Important Execution Methods

| Method | Typical use | Return value |
|---|---|---|
| `executeQuery()` | SELECT queries | `ResultSet` |
| `executeUpdate()` | INSERT, UPDATE, DELETE | Number of affected rows |
| `execute()` | Statements that may return different result types | Boolean indicating whether the first result is a `ResultSet` |

For `execute()`, use the associated JDBC methods to inspect and retrieve results.

## 12. CallableStatement

`CallableStatement` is used to call stored procedures in a database.

Example syntax:

```java
CallableStatement cs =
    con.prepareCall("{call get_students()}");
```

A stored procedure is a database-side program that can contain SQL statements and control logic.

For procedures with parameters, JDBC supports input, output, and input/output parameters.

## 13. JDBC Transactions

A transaction groups database operations into a unit of work.

By default, a JDBC connection normally uses auto-commit mode, meaning individual statements are committed automatically.

To manage a transaction manually:

```java
con.setAutoCommit(false);

try {
    // Execute related database operations here.

    con.commit();
} catch (Exception e) {
    con.rollback();
    throw e;
}
```

### Important methods

- `setAutoCommit(false)` — disables automatic commits.
- `commit()` — makes the transaction's changes permanent.
- `rollback()` — rolls back uncommitted changes.

**Exam point:** Transactions help maintain consistency when multiple related database operations must succeed or fail together.

## 14. Exception Handling in JDBC

Database operations can throw `SQLException`.

It can provide information such as:

- An error message.
- An SQL state.
- A vendor-specific error code.

Example:

```java
catch (SQLException e) {
    System.out.println(e.getMessage());
    System.out.println(e.getSQLState());
    System.out.println(e.getErrorCode());
}
```

Always handle errors appropriately and close database resources.

## 15. Closing JDBC Resources

Resources such as `Connection`, `Statement`, and `ResultSet` should be closed when they are no longer needed.

**Try-with-resources** is the recommended approach for JDBC resources that implement `AutoCloseable`.

```java
try (Connection con = DriverManager.getConnection(url, user, password);
     Statement st = con.createStatement();
     ResultSet rs = st.executeQuery("SELECT * FROM students")) {

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }
}
```

Resources are closed automatically when the try block exits, including when an exception occurs.

## 16. SQL Injection

SQL injection occurs when untrusted input is incorporated into SQL in a way that changes the intended query.

Unsafe example:

```java
String sql =
    "SELECT * FROM users WHERE username = '" + username + "'";
```

Safer approach:

```java
String sql =
    "SELECT * FROM users WHERE username = ?";

PreparedStatement ps = con.prepareStatement(sql);
ps.setString(1, username);
```

Use parameterized queries for values. Placeholders generally cannot be used to substitute SQL identifiers such as table names or column names.

## 17. JDBC vs ODBC

| JDBC | ODBC |
|---|---|
| Java Database Connectivity | Open Database Connectivity |
| Java-oriented API | Language-independent database access standard |
| Used by Java programs | Used by applications written in various languages |
| Works through JDBC drivers | Works through ODBC drivers |

The old JDBC-ODBC Bridge driver was removed from the JDK in Java 8.

## 18. JDBC in Modern Java Applications

JDBC remains an important foundation for database access. Frameworks such as Spring Boot can simplify database operations using:

- Spring JDBC and `JdbcTemplate`
- JPA and Hibernate
- Spring Data repositories

These technologies build on or complement JDBC; they do not make understanding JDBC concepts irrelevant.

For Placelyt, Spring Boot handles much of the database-access plumbing, but JDBC concepts still help you understand connections, SQL queries, transactions, and database errors.

## 19. Quick Revision

- JDBC stands for Java Database Connectivity.
- Main package: `java.sql`.
- `DriverManager` obtains connections.
- `Connection` represents a database connection.
- `Statement` executes SQL.
- `PreparedStatement` supports parameterized SQL.
- `CallableStatement` calls stored procedures.
- `ResultSet` contains query results.
- `executeQuery()` is typically used for SELECT.
- `executeUpdate()` is typically used for INSERT, UPDATE, and DELETE.
- JDBC parameter indexes start at 1.
- `commit()` commits a transaction.
- `rollback()` rolls back uncommitted changes.
- Try-with-resources closes JDBC resources automatically.
- Prepared statements help protect against SQL injection.
- Type 4 drivers are pure Java drivers.
