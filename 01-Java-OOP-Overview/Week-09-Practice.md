# Week 9 — Java Multithreading Practice

## Part A: Predict the Output

### Q1. Calling `run()` directly

```java
class MyThread extends Thread {
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.run();
    }
}
```

**Answer:**
```text
main
```

**Explanation:** Calling `run()` directly does not create a new thread. It executes as an ordinary method on the main thread.

---

### Q2. Calling `start()`

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();
        System.out.println("Main");
    }
}
```

**Answer:** Either of these orders is possible:

```text
Child
Main
```

or

```text
Main
Child
```

**Explanation:** Once the child thread starts, the scheduler determines when each thread executes. Do not assume a fixed output order unless synchronization establishes one.

---

### Q3. Starting the same thread twice

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();
        t.start();
    }
}
```

**Answer:** The second call to `start()` throws `IllegalThreadStateException`.

The first thread may print `Running` before the exception, but the exact timing of the print relative to the exception is not guaranteed.

**Exam trap:** A terminated thread also cannot be restarted.

---

### Q4. `sleep()` and the current thread

```java
class MyThread extends Thread {
    public void run() {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println("Child finished");
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        MyThread t = new MyThread();
        t.start();
        t.join();
        System.out.println("Main finished");
    }
}
```

**Answer:**
```text
Child finished
Main finished
```

**Explanation:** `join()` makes the main thread wait until the child thread terminates.

---

### Q5. `join()` versus no `join()`

Consider:

```java
Thread t = new Thread(() -> System.out.println("A"));
t.start();
System.out.println("B");
```

**Answer:** Either `A` then `B`, or `B` then `A`.

If `t.join()` is placed after `t.start()` and before printing `B`, then `A` is printed before `B`.

---

### Q6. Thread priority

```java
Thread t1 = new Thread(() -> System.out.println("One"));
Thread t2 = new Thread(() -> System.out.println("Two"));

t1.setPriority(Thread.MAX_PRIORITY);
t2.setPriority(Thread.MIN_PRIORITY);

t1.start();
t2.start();
```

**Answer:** The output order is not guaranteed.

**Explanation:** Thread priority is not a guarantee that one thread will execute or finish before another.

---

### Q7. `finally` is unrelated to scheduling

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("Child");
        });

        t.start();

        try {
            t.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            System.out.println("Finally");
        }
    }
}
```

**Answer:**
```text
Child
Finally
```

**Explanation:** If `join()` completes normally, the child has terminated before the `finally` block executes.

---

## Part B: Synchronization

### Q8. Why can `count++` be unsafe?

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

**Answer:** Multiple threads can read the same old value and overwrite each other's updates. The final count can be less than the total number of increments.

**Solution:** Protect the increment using synchronization or use an appropriate atomic counter.

---

### Q9. Synchronized method

```java
class Counter {
    private int count = 0;

    synchronized void increment() {
        count++;
    }

    synchronized int getCount() {
        return count;
    }
}
```

**Question:** Can two threads execute `increment()` simultaneously on the same `Counter` object?

**Answer:** No. Only one thread at a time can hold that object's monitor.

**Important:** Two different `Counter` objects have different instance monitors.

---

### Q10. Instance synchronization versus static synchronization

Which monitor is acquired?

```java
class Demo {
    synchronized void instanceMethod() {
    }

    static synchronized void staticMethod() {
    }
}
```

**Answer:**
- `instanceMethod()` acquires the monitor of the current `Demo` instance.
- `staticMethod()` acquires the monitor of the `Demo.class` object.

They are different monitors, so the two methods do not automatically block each other.

---

## Part C: `wait()`, `notify()`, and `sleep()`

### Q11. Does `sleep()` release a lock?

**Question:** A thread calls `Thread.sleep(1000)` inside a synchronized block. Does it release the monitor?

**Answer:** No. The thread retains the monitor while sleeping.

### Q12. Does `wait()` release a lock?

**Question:** A thread calls `lock.wait()` while holding `lock`'s monitor. What happens?

**Answer:** The thread releases that monitor and waits. After being notified or otherwise awakened, it must reacquire the monitor before continuing.

### Q13. Calling `wait()` incorrectly

```java
Object lock = new Object();
lock.wait();
```

**Answer:** If executed without owning `lock`'s monitor, it throws `IllegalMonitorStateException`.

**Correct form:**

```java
synchronized (lock) {
    lock.wait();
}
```

In real code, wait inside a loop that checks a condition.

### Q14. `notify()` versus `notifyAll()`

**Answer:**
- `notify()` wakes one arbitrarily selected waiting thread.
- `notifyAll()` wakes all threads waiting on that object's monitor.
- Neither method immediately transfers the monitor to the awakened thread.
- The awakened thread must reacquire the monitor before proceeding.

---

## Part D: Thread Lifecycle and Concepts

### Q15. Identify the state

Match each situation with the most appropriate Java thread state.

| Situation | State |
|---|---|
| Thread object created but not started | `NEW` |
| Waiting to acquire a synchronized monitor | `BLOCKED` |
| Calling `sleep(1000)` | `TIMED_WAITING` |
| Calling `wait()` without a timeout | `WAITING` |
| `run()` has finished | `TERMINATED` |
| Ready to execute or currently executing | `RUNNABLE` |

### Q16. `isAlive()`

```java
Thread t = new Thread(() -> System.out.println("Hello"));
System.out.println(t.isAlive());
t.start();
```

**Answer:** The first printed value is `false`, because the thread has not started yet.

Immediately after `start()`, `isAlive()` may return either `true` or `false` depending on whether the thread has already completed.

### Q17. Daemon thread

**Question:** What happens if only daemon threads remain after all non-daemon threads finish?

**Answer:** The JVM may exit. Daemon threads should not be relied on to complete critical tasks or perform guaranteed cleanup.

Remember to call `setDaemon(true)` before `start()`.

### Q18. `volatile`

```java
volatile boolean running = true;
```

**Question:** Does `volatile` make `running = false` visible to other threads? Does it make `count++` atomic?

**Answer:**
- It provides visibility and ordering guarantees for reads and writes of the volatile variable.
- It does not make a compound operation such as `count++` atomic.

---

## Part E: Write the Programs

### Q19. Create two threads using `Runnable`

Write a program that creates two threads, each printing a message three times.

**Solution:**

```java
class MyTask implements Runnable {
    private final String message;

    MyTask(String message) {
        this.message = message;
    }

    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println(message + " " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyTask("Task A"));
        Thread t2 = new Thread(new MyTask("Task B"));

        t1.start();
        t2.start();
    }
}
```

**Note:** The lines from the two threads may interleave in different orders.

### Q20. Use synchronization to count safely

```java
class Counter {
    private int count = 0;

    synchronized void increment() {
        count++;
    }

    synchronized int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter c = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                c.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                c.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println(c.getCount());
    }
}
```

**Output:**
```text
20000
```

**Why?** The synchronized increment protects the shared count, and `join()` ensures both threads finish before the final count is read.

---

## Part F: NPTEL Quick Test

Try these without looking at the answers.

1. Which method actually starts a new thread?
2. Is `run()` a method of `Thread`?
3. Can a Java thread be started twice?
4. Which state represents waiting to acquire a monitor?
5. Does `sleep()` release a monitor lock?
6. Does `wait()` release the relevant monitor?
7. Which object does a static synchronized method lock?
8. Does thread priority guarantee execution order?
9. Is `volatile int count` sufficient to make `count++` thread-safe?
10. Which interface is commonly implemented to define a task for a thread?
11. Which method waits for another thread to terminate?
12. Which methods are defined in `Object`: `wait()`, `notify()`, or both?
13. What is a race condition?
14. What is a deadlock?
15. What does `notifyAll()` do?

### Answer Key

1. `start()`
2. Yes.
3. No.
4. `BLOCKED`
5. No.
6. Yes.
7. The associated `Class` object's monitor.
8. No.
9. No.
10. `Runnable`
11. `join()`
12. Both.
13. A situation where the result depends on uncontrolled timing or interleaving of operations.
14. A situation where threads wait indefinitely for resources held by one another.
15. Wakes all threads waiting on that object's monitor; they must compete to reacquire it.

---
