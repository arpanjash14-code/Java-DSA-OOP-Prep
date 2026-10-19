# Week 11 — Java Networking & Applets: Practice

## Part A — Conceptual Questions

Answer each question in your own words.

1. What is Java networking? Which package provides Java's networking classes?
2. Differentiate between TCP and UDP.
3. What is the purpose of the `InetAddress` class?
4. Differentiate between `Socket` and `ServerSocket`.
5. What is the purpose of the `accept()` method?
6. What are `DatagramSocket` and `DatagramPacket` used for?
7. Differentiate between `URL` and `URLConnection`.
8. What is an applet? Why does it not require a `main()` method?
9. Explain the applet lifecycle methods:
   - `init()`
   - `start()`
   - `stop()`
   - `destroy()`
10. Why are Java applets considered obsolete? What is a modern alternative for interactive web applications?

## Part B — Differentiate Between

Write at least three differences for each.

1. TCP vs UDP
2. `Socket` vs `ServerSocket`
3. `InputStream` vs `OutputStream`
4. `URL` vs `URLConnection`
5. Application vs Applet

## Part C — Predict the Output

### Question 1: Applet lifecycle

Consider this historical applet example:

```java
import java.applet.Applet;

public class DemoApplet extends Applet {
    public void init() {
        System.out.println("Init");
    }

    public void start() {
        System.out.println("Start");
    }

    public void stop() {
        System.out.println("Stop");
    }

    public void destroy() {
        System.out.println("Destroy");
    }
}
```

**Questions:**

1. Which method is normally called first?
2. Which method may be called when the applet becomes active?
3. Which method may be called when the applet becomes inactive?
4. Is the complete lifecycle guaranteed to execute in every situation?

### Question 2: TCP server

```java
import java.net.ServerSocket;
import java.net.Socket;

public class ServerDemo {
    public static void main(String[] args) throws Exception {
        ServerSocket server = new ServerSocket(5000);
        System.out.println("Waiting");

        Socket client = server.accept();
        System.out.println("Connected");
    }
}
```

**Questions:**

1. What is printed before a client connects?
2. Does `accept()` return immediately?
3. What happens if no client connects?
4. Why should the server and client sockets be closed when no longer needed?

*Note:* This program blocks at `accept()` until a client connects or an error occurs.

### Question 3: TCP vs UDP

A program sends a message using UDP.

**Questions:**

1. Does UDP guarantee delivery?
2. Does UDP guarantee that packets arrive in order?
3. Must a UDP program establish a TCP-style connection first?

## Part D — Coding Exercises

### Exercise 1: Display the local host address

Write a Java program using `InetAddress` to display the local host name and IP address.

**Hint:** Use `InetAddress.getLocalHost()`.

### Exercise 2: TCP client

Write a TCP client that:

1. Connects to `localhost` on port `5000`.
2. Sends a text message to the server.
3. Uses a `Socket` and an output stream.
4. Closes the socket and stream safely.

### Exercise 3: TCP server

Write a TCP server that:

1. Listens on port `5000`.
2. Accepts one client.
3. Reads a line of text from the client.
4. Prints the received message.
5. Closes its resources safely.

### Exercise 4: UDP communication

Write a program that creates a `DatagramPacket` containing a short message and sends it using `DatagramSocket`.

Identify the destination IP address and port.

### Exercise 5: URL information

Write a program that creates a `URL` object and displays its protocol, host, port, and path.

Use a valid URL, such as:

```java
https://example.com:443/docs/index.html
```

### Exercise 6: Applet lifecycle

Write a historical applet class that prints a message from each lifecycle method. Explain the usual order of calls and why modern browsers cannot be relied upon to run it.

## Part E — MCQs

**1. Which package contains Java's core networking classes?**

A. `java.io`  
B. `java.net`  
C. `java.sql`  
D. `java.awt`

**2. Which protocol provides reliable, ordered byte-stream delivery?**

A. UDP  
B. IP  
C. TCP  
D. DNS

**3. Which class is used by a TCP server to listen for incoming connections?**

A. `Socket`  
B. `ServerSocket`  
C. `DatagramPacket`  
D. `InetAddress`

**4. Which method waits for an incoming TCP connection?**

A. `connect()`  
B. `send()`  
C. `accept()`  
D. `receive()`

**5. Which class represents an IP address and hostname information?**

A. `InetAddress`  
B. `URLClassLoader`  
C. `ServerSocket`  
D. `PrintWriter`

**6. Which pair is commonly used for UDP communication?**

A. `Socket` and `ServerSocket`  
B. `DatagramSocket` and `DatagramPacket`  
C. `URL` and `ServerSocket`  
D. `Thread` and `Runnable`

**7. Which applet lifecycle method is generally called once for initialization?**

A. `start()`  
B. `stop()`  
C. `init()`  
D. `destroy()`

**8. Which statement about UDP is correct?**

A. It always guarantees packet delivery.  
B. It always guarantees packet ordering.  
C. It is connectionless and does not guarantee delivery.  
D. It can only send text.

**9. What does `URLConnection` provide?**

A. A way to work with a connection to a resource identified by a URL.  
B. A way to create Java classes.  
C. A replacement for every TCP socket.  
D. A mechanism for inheriting classes.

**10. What is the current status of Java applets in modern browsers?**

A. They are the standard way to build web applications.  
B. They are supported by all modern browsers by default.  
C. They are obsolete and are not supported by mainstream modern browsers.  
D. They replace Java Servlets.

## Part F — Answer Key

Try the questions before checking these answers.

1. **B** — `java.net`
2. **C** — TCP
3. **B** — `ServerSocket`
4. **C** — `accept()`
5. **A** — `InetAddress`
6. **B** — `DatagramSocket` and `DatagramPacket`
7. **C** — `init()`
8. **C** — UDP is connectionless and does not guarantee delivery.
9. **A** — It provides access to a resource through a URL connection.
10. **C** — Applets are obsolete in mainstream modern browsers.
