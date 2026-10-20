# Week 12 — JDBC & Database Connectivity: Practice

## Part A — Conceptual Questions

Answer these without checking your notes.

1. What is JDBC? Which package contains its core interfaces and classes?
2. Explain the four types of JDBC drivers.
3. What is the role of `DriverManager`?
4. Differentiate between `Connection`, `Statement`, and `ResultSet`.
5. What is `PreparedStatement`, and why is it preferred for parameterized SQL?
6. What is `CallableStatement` used for?
7. Differentiate between `executeQuery()`, `executeUpdate()`, and `execute()`.
8. What does the `next()` method do in a `ResultSet`?
9. What is auto-commit mode? How do `commit()` and `rollback()` work?
10. What is SQL injection? How can parameterized queries help prevent it?
11. Why should JDBC resources be closed?
12. What is try-with-resources, and why is it useful in JDBC?

## Part B — Differentiate Between

Write at least three differences for each.

1. `Statement` vs `PreparedStatement`
2. `executeQuery()` vs `executeUpdate()`
3. `commit()` vs `rollback()`
4. JDBC vs ODBC
5. Type 2 vs Type 4 JDBC drivers

## Part C — Predict the Output

### Question 1: Affected rows

```java
int rows = statement.executeUpdate(
    "UPDATE students SET name = 'Riya' WHERE id = 2"
);
System.out.println(rows);
```

Assume exactly one existing row matches `id = 2`, and its name changes.

What is printed?

### Question 2: ResultSet cursor

Suppose a query returns three rows. Consider:

```java
while (rs.next()) {
    System.out.println(rs.getString("name"));
}
```

1. How many times does the loop execute?
2. What does `rs.next()` return after the final row has been processed?
3. Where is the cursor initially positioned when a typical `ResultSet` is created?

### Question 3: Parameter indexes

```java
PreparedStatement ps = con.prepareStatement(
    "SELECT * FROM students WHERE id = ? AND name = ?"
);

ps.setInt(1, 10);
ps.setString(2, "Amit");
```

1. Which value replaces the first placeholder?
2. Which value replaces the second placeholder?
3. What would happen if you tried to bind parameter index `0`?

### Question 4: Execution methods

Identify the most appropriate method for each operation:

1. Retrieve student records.
2. Insert a student record.
3. Update a student's name.
4. Execute a statement whose result type may vary.

Choose from `executeQuery()`, `executeUpdate()`, and `execute()`.

## Part D — Coding Exercises

### Exercise 1: Database connection

Write a Java program that connects to a MySQL database named `college` using `DriverManager`.

Requirements:

- Use a JDBC URL.
- Obtain a `Connection`.
- Print a success message if the connection is established.
- Handle `SQLException`.
- Close the connection safely.

### Exercise 2: Insert a record

Assume this table exists:

```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

Write a program that inserts one record using `PreparedStatement`.

Requirements:

- Use placeholders.
- Bind the ID and name with appropriate setter methods.
- Execute the insert.
- Print the number of affected rows.

### Exercise 3: Retrieve records

Write a program that selects all students and prints their IDs and names.

Requirements:

- Use `executeQuery()`.
- Iterate through the `ResultSet` using `next()`.
- Use try-with-resources.

### Exercise 4: Update a record

Write a program that updates a student's name based on their ID using `PreparedStatement`.

Explain why parameterized SQL is safer than concatenating untrusted input into the query.

### Exercise 5: Delete a record

Write a program that deletes a student by ID.

Print the number of rows affected by the delete operation.

### Exercise 6: Transaction handling

Write a program outline that:

1. Disables auto-commit.
2. Performs two related database updates.
3. Commits if both operations succeed.
4. Rolls back if either operation fails.

Explain why transactions are useful.

## Part E — MCQs

**1. Which package contains JDBC's core interfaces?**

A. `java.net`  
B. `java.sql`  
C. `java.awt`  
D. `java.util`

**2. Which method typically executes a SELECT query?**

A. `executeUpdate()`  
B. `executeQuery()`  
C. `commit()`  
D. `rollback()`

**3. Which JDBC interface supports parameterized SQL statements?**

A. `ResultSet`  
B. `Connection`  
C. `PreparedStatement`  
D. `InetAddress`

**4. JDBC parameter indexes normally begin at:**

A. 0  
B. 1  
C. -1  
D. 2

**5. Which method moves the `ResultSet` cursor to the next row?**

A. `move()`  
B. `next()`  
C. `advance()`  
D. `fetchNext()`

**6. Which method commits a transaction?**

A. `close()`  
B. `execute()`  
C. `commit()`  
D. `rollback()`

**7. Which JDBC interface is used to call stored procedures?**

A. `CallableStatement`  
B. `PreparedStatement`  
C. `ResultSet`  
D. `DriverManager`

**8. Which driver type is a pure Java driver that communicates directly using the database protocol?**

A. Type 1  
B. Type 2  
C. Type 3  
D. Type 4

**9. What is the primary purpose of `rollback()`?**

A. Close the database server  
B. Undo uncommitted transaction changes  
C. Create a database  
D. Retrieve query results

**10. Which approach is recommended for closing JDBC resources safely?**

A. Never close resources  
B. Use `System.exit()`  
C. Use try-with-resources  
D. Use only `finalize()`

## Part F — Answer Key

Try all questions first.

### Part C

1. **Question 1:** `1`
2. **Question 2:**
   - The loop executes three times.
   - `rs.next()` returns `false` after the final row.
   - The cursor initially sits before the first row.
3. **Question 3:**
   - First placeholder: `10`
   - Second placeholder: `"Amit"`
   - Parameter index `0` is invalid; JDBC parameter indexes start at `1`.
4. **Question 4:**
   - Retrieve records: `executeQuery()`
   - Insert: `executeUpdate()`
   - Update: `executeUpdate()`
   - Variable result type: `execute()`

### Part E

1. **B** — `java.sql`
2. **B** — `executeQuery()`
3. **C** — `PreparedStatement`
4. **B** — `1`
5. **B** — `next()`
6. **C** — `commit()`
7. **A** — `CallableStatement`
8. **D** — Type 4
9. **B** — Undo uncommitted transaction changes
10. **C** — Try-with-resources
