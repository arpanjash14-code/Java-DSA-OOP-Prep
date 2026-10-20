# Week 9 — Multithreading in Java

## 1. What Is a Thread?

A **thread** is a lightweight unit of execution within a process. Multiple threads can execute concurrently within the same Java program.

Examples:
- One thread downloads a file while another updates the interface.
- One thread processes data while another handles user input.

**Multitasking** refers to running multiple tasks, while **multithreading** refers to executing multiple threads within a process.

### Concurrency vs Parallelism

- **Concurrency:** Multiple tasks make progress over overlapping periods; execution may be interleaved.
- **Parallelism:** Multiple tasks execute at the same time, typically on different CPU cores.

Multithreading does not guarantee parallel execution.

## 2. Creating Threads

Java provides two common ways to define work for a thread.

### Method 1: Extend `Thread`

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();
    }
}
```

`start()` asks the JVM to start a new thread, which then invokes `run()`.

### Method 2: Implement `Runnable`

```java
class MyTask implements Runnable {
    public void run() {
        System.out.println("Task is running");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyTask());
        t.start();
    }
}
```

**Why is `Runnable` often preferred?**

Java supports single class inheritance. Implementing `Runnable` lets your class extend another class while still defining work for a thread.

### Comparison

| `Thread` | `Runnable` |
|---|---|
| Extend the `Thread` class | Implement the `Runnable` interface |
| The class cannot extend another class | The class can extend another class |
| Combines task definition with thread object | Separates the task from the thread |

## 3. `start()` vs `run()` — Very Important

- `start()` starts a new thread of execution.
- `run()` contains the work performed by the thread.
- Calling `run()` directly is an ordinary method call; it does **not** start a new thread.
- A thread cannot be started more than once. Attempting to restart a terminated thread throws `IllegalThreadStateException`.

Example:

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
        t.start();
    }
}
```

The first printed name is `main`, because `run()` was called directly. The second name is typically `Thread-0`, because `start()` starts a new thread.

## 4. Thread Lifecycle

Java's `Thread.State` defines these states:

1. **NEW:** Thread object created, but not started.
2. **RUNNABLE:** Eligible to run or currently running.
3. **BLOCKED:** Waiting to acquire a monitor lock.
4. **WAITING:** Waiting indefinitely for another thread's action.
5. **TIMED_WAITING:** Waiting for a limited time.
6. **TERMINATED:** Execution has finished.

Important distinctions:

- Java's `RUNNABLE` state includes a thread that is actually running; Java does not expose a separate `RUNNING` state.
- Calling `start()` moves a new thread into the runnable lifecycle.
- `sleep()` generally produces `TIMED_WAITING`.
- `wait()` without a timeout produces `WAITING`.
- `join()` without a timeout generally puts the calling thread into `WAITING`.
- Waiting to acquire a synchronized monitor lock produces `BLOCKED`.

## 5. Important Thread Methods

| Method | Purpose |
|---|---|
| `start()` | Starts a new thread |
| `run()` | Defines the work to perform |
| `sleep(ms)` | Pauses the current thread for a specified duration |
| `join()` | Waits for another thread to terminate |
| `interrupt()` | Requests interruption of a thread |
| `isAlive()` | Checks whether a thread has started and not yet terminated |
| `currentThread()` | Returns the currently executing thread |
| `getName()` | Returns the thread's name |
| `setName()` | Changes the thread's name |

### Example: `sleep()`

```java
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 3; i++) {
            System.out.println(i);

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                return;
            }
        }
    }
}
```

`Thread.sleep()` is a static method. It pauses the **currently executing thread**, not necessarily the object on which it is called.

**Exam trap:** `sleep()` does not release any monitor lock held by the thread.

### Example: `join()`

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Child thread");
    }
}

public class Main {
    public static void main(String[] args)
            throws InterruptedException {
        MyThread t = new MyThread();
        t.start();
        t.join();

        System.out.println("Main thread");
    }
}
```

Output:

```text
Child thread
Main thread
```

The call to `join()` makes the main thread wait until `t` terminates.

## 6. Thread Priorities

Java thread priorities range from 1 to 10.

- `Thread.MIN_PRIORITY` = 1
- `Thread.NORM_PRIORITY` = 5
- `Thread.MAX_PRIORITY` = 10

Example:

```java
t.setPriority(Thread.MAX_PRIORITY);
```

**Important:** Priority is a scheduling hint, not a guarantee of execution order. Do not assume the highest-priority thread must finish first.

## 7. Synchronization

When multiple threads access shared mutable data, their operations can interfere with each other.

This may cause a **race condition**.

### Example: Race condition

```java
class Counter {
    int count = 0;

    void increment() {
        count++;
    }
}
```

Although `count++` looks like one operation, it involves reading, modifying, and writing a value. Concurrent increments can be lost.

### Using a synchronized method

```java
class Counter {
    private int count = 0;

    synchronized void increment() {
        count++;
    }

    int getCount() {
        return count;
    }
}
```

For calls on the same `Counter` instance, only one thread at a time can execute a synchronized instance method while holding that instance's monitor.

### Synchronized block

```java
synchronized (this) {
    count++;
}
```

A synchronized block protects the critical section using the monitor of the specified object.

### Static synchronization

```java
static synchronized void display() {
    System.out.println("Static synchronized");
}
```

A static synchronized method locks the monitor associated with the class's `Class` object, rather than an individual instance.

### Key points

- `synchronized` provides mutual exclusion for the same monitor.
- It also provides memory-visibility guarantees when threads synchronize on the same monitor.
- Synchronizing unrelated objects does not automatically protect the same shared variable.
- Excessive synchronization can reduce performance or lead to deadlocks.

## 8. Inter-Thread Communication: `wait()`, `notify()`, `notifyAll()`

These methods are defined in `Object`.

- `wait()` causes the current thread to wait and releases the monitor associated with that object.
- `notify()` wakes one thread waiting on that object's monitor, if one exists.
- `notifyAll()` wakes all threads waiting on that object's monitor so they can compete to reacquire it.

Example:

```java
synchronized (lock) {
    while (!condition) {
        lock.wait();
    }

    // Continue when the condition is satisfied.
}
```

The condition is checked in a `while` loop because a thread can wake up without the condition being true, and another thread may change the condition before the awakened thread reacquires the lock.

**Exam traps:**

- `wait()`, `notify()`, and `notifyAll()` must be called by a thread that owns the object's monitor; otherwise, `IllegalMonitorStateException` is thrown.
- `wait()` releases the relevant monitor while waiting.
- `sleep()` does not release a monitor.
- `notify()` does not immediately transfer the lock to the notified thread.

## 9. Deadlock

A deadlock occurs when threads wait indefinitely for resources held by one another.

Example scenario:

- Thread A holds Lock 1 and waits for Lock 2.
- Thread B holds Lock 2 and waits for Lock 1.

Neither can proceed.

Common prevention strategies:
- Acquire locks in a consistent global order.
- Avoid unnecessary nested locking.
- Keep synchronized sections short.
- Consider higher-level concurrency utilities where appropriate.

## 10. Daemon Threads

Daemon threads support background work for ordinary user threads.

```java
Thread t = new Thread(() -> {
    System.out.println("Background task");
});

t.setDaemon(true);
t.start();
```

`setDaemon(true)` must be called before the thread starts.

The JVM may exit when all started non-daemon threads have terminated, even if daemon threads are still executing.

Daemon status does not mean a thread is automatically low-priority or guaranteed to finish its task.

## 11. Thread Safety and Atomic Operations

A class is thread-safe when its behavior remains correct under concurrent use according to its contract.

Useful tools include:
- `synchronized`
- `volatile`
- Atomic classes such as `AtomicInteger`
- Concurrent collections such as `ConcurrentHashMap`

### What does `volatile` do?

A volatile variable provides visibility and ordering guarantees for reads and writes of that variable.

```java
private volatile boolean running = true;
```

It does **not** make compound operations such as `count++` atomic.

For atomic increments, consider:

```java
import java.util.concurrent.atomic.AtomicInteger;

AtomicInteger count = new AtomicInteger(0);
count.incrementAndGet();
```

## 12. Modern Alternative: Executor Framework

Creating a new thread for every task is not always efficient. Executors help manage task execution and thread reuse.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor =
                Executors.newFixedThreadPool(2);

        executor.submit(() -> System.out.println("Task 1"));
        executor.submit(() -> System.out.println("Task 2"));

        executor.shutdown();
    }
}
```

The tasks may execute in either order. A fixed thread pool limits the number of worker threads.

For traditional NPTEL questions, focus first on `Thread`, `Runnable`, lifecycle, synchronization, and inter-thread communication.

## 13. Rapid Revision

| Concept | Remember |
|---|---|
| `start()` | Starts a new thread |
| Direct `run()` call | Ordinary method call |
| `sleep()` | Pauses current thread; does not release monitor locks |
| `wait()` | Waits and releases the object's monitor |
| `join()` | Waits for another thread to terminate |
| `synchronized` instance method | Locks the instance monitor |
| `static synchronized` method | Locks the class monitor |
| `notify()` | Wakes one waiting thread |
| `notifyAll()` | Wakes all threads waiting on that monitor |
| Thread priority | Does not guarantee execution order |
| `volatile` | Visibility/order, not compound-operation atomicity |
| Daemon thread | Background thread; does not keep JVM alive by itself |
