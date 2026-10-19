# Week 11 — Java Networking & Applets

## 1. Introduction to Java Networking

Java networking allows programs to communicate across computers using network protocols.

The main networking classes are in the `java.net` package.

Common applications include:
- Client-server communication
- Sending and receiving data over a network
- Downloading resources
- Communicating with remote services

## 2. Important Classes in `java.net`

| Class | Purpose |
|---|---|
| `InetAddress` | Represents an IP address and provides host/address lookup methods |
| `URL` | Represents a Uniform Resource Locator |
| `URLConnection` | Represents a connection to a resource identified by a URL |
| `Socket` | TCP client-side communication endpoint |
| `ServerSocket` | Listens for incoming TCP connections |
| `DatagramSocket` | Sends and receives UDP datagrams |
| `DatagramPacket` | Represents a UDP datagram |
| `UnknownHostException` | Indicates that a host's IP address could not be determined |

## 3. IP Address and Port Number

An **IP address** identifies a network interface or host within an IP network.

A **port number** identifies a communication endpoint associated with an application or service.

For example:

```text
192.168.1.10:8080
```

Here:
- `192.168.1.10` is the IP address.
- `8080` is the port number.

A port number is not an IP address. Together, an IP address and port help identify a network endpoint.

## 4. TCP vs UDP

TCP and UDP are transport-layer protocols.

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Provides a reliable, ordered byte stream | Provides datagrams without delivery or ordering guarantees |
| Uses retransmission and flow/congestion control | Does not provide TCP-style reliability |
| Common for web connections and file transfers | Common for latency-sensitive applications and some real-time traffic |
| Java classes: `Socket`, `ServerSocket` | Java classes: `DatagramSocket`, `DatagramPacket` |

**Exam trap:** TCP provides a byte stream, not message boundaries. An application protocol must define how individual messages are framed.

## 5. The `InetAddress` Class

`InetAddress` represents an IP address and provides methods for resolving hostnames.

Example:

```java
import java.net.InetAddress;

public class Main {
    public static void main(String[] args) throws Exception {
        InetAddress address =
                InetAddress.getByName("localhost");

        System.out.println(address.getHostName());
        System.out.println(address.getHostAddress());
    }
}
```

The exact textual address can vary by system configuration. `localhost` typically resolves to a loopback address.

Important methods:

| Method | Purpose |
|---|---|
| `getByName(String host)` | Resolves a hostname or address |
| `getAllByName(String host)` | Returns all resolved addresses |
| `getHostName()` | Returns the hostname associated with the address |
| `getHostAddress()` | Returns the textual IP address |

## 6. TCP Client-Server Communication

TCP establishes a connection before application data is exchanged.

The usual sequence is:

1. The server creates a `ServerSocket`.
2. The server calls `accept()` and waits for a client.
3. The client creates a `Socket` connecting to the server.
4. Both sides exchange data using input and output streams.
5. Both sides close their resources.

### TCP server example

```java
import java.io.*;
import java.net.*;

public class TCPServer {
    public static void main(String[] args) throws IOException {
        try (ServerSocket server =
                     new ServerSocket(5000)) {

            System.out.println("Waiting for client...");

            try (Socket socket = server.accept();
                 BufferedReader in = new BufferedReader(
                         new InputStreamReader(
                                 socket.getInputStream()));
                 PrintWriter out = new PrintWriter(
                         socket.getOutputStream(), true)) {

                String message = in.readLine();
                System.out.println("Client: " + message);

                out.println("Message received");
            }
        }
    }
}
```

### TCP client example

```java
import java.io.*;
import java.net.*;

public class TCPClient {
    public static void main(String[] args) throws IOException {
        try (Socket socket =
                     new Socket("localhost", 5000);
             BufferedReader in = new BufferedReader(
                     new InputStreamReader(
                             socket.getInputStream()));
             PrintWriter out = new PrintWriter(
                     socket.getOutputStream(), true)) {

            out.println("Hello Server");

            String response = in.readLine();
            System.out.println("Server: " + response);
        }
    }
}
```

### How to run

Compile both classes:

```text
javac TCPServer.java TCPClient.java
```

Start the server first in one terminal:

```text
java TCPServer
```

Then run the client in another terminal:

```text
java TCPClient
```

Expected client output:

```text
Server: Message received
```

Expected server output includes:

```text
Waiting for client...
Client: Hello Server
```

**Important:** These examples use a newline-delimited text protocol. `readLine()` waits until it receives a line terminator or reaches end-of-stream, so the sender must send a newline or close the output stream appropriately.

## 7. Important Socket Methods

| Method | Purpose |
|---|---|
| `new Socket(host, port)` | Connects to a TCP server |
| `new ServerSocket(port)` | Creates a server listening socket |
| `accept()` | Waits for a client and returns a connected socket |
| `getInputStream()` | Gets the socket input stream |
| `getOutputStream()` | Gets the socket output stream |
| `getPort()` | Returns the remote port |
| `getLocalPort()` | Returns the local port |
| `close()` | Closes the socket |

**Exam trap:** `ServerSocket.accept()` blocks until a connection is accepted or the operation fails.

## 8. UDP Communication

UDP sends independent datagrams without establishing a TCP-style connection.

Java uses:
- `DatagramSocket` to send and receive datagrams.
- `DatagramPacket` to represent the datagram's data and addressing information.

A UDP receiver typically creates a datagram socket on a port and calls `receive()`. A sender creates a packet containing the data, destination address, and destination port, then calls `send()`.

UDP does not guarantee that a packet will arrive, arrive only once, or arrive in order.

## 9. URL and URLConnection

A URL identifies a resource, such as a web page or file.

Example:

```java
import java.net.URL;

public class Main {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://example.com");

        System.out.println(url.getProtocol());
        System.out.println(url.getHost());
        System.out.println(url.getPath());
    }
}
```

Typical output:

```text
https
example.com

```

The path is empty for this URL. URL parsing and connection behavior can vary with the URL and environment.

Useful methods:
- `getProtocol()`
- `getHost()`
- `getPort()`
- `getPath()`
- `openConnection()`

`URLConnection` provides access to a resource connection and its associated metadata and streams. Network access can fail due to DNS, TLS, permissions, firewalls, or remote-server errors.

## 10. Applets: Historical Syllabus Topic

An **applet** was a small Java program intended to run within an applet-enabled environment, historically a web browser or applet viewer.

Applets are obsolete: modern browsers do not support the Java browser plug-in, and modern Java versions no longer provide the old applet deployment model. Learn the lifecycle for exams, but do not use applets for new projects.

### Traditional applet lifecycle

The classic lifecycle methods are:

1. `init()` — called when the applet is initialized.
2. `start()` — called when the applet becomes active.
3. `paint(Graphics g)` — draws the applet's visual content when painting is requested.
4. `stop()` — called when the applet becomes inactive.
5. `destroy()` — called when the applet is being unloaded.

`paint(Graphics g)` is an AWT painting callback rather than one of the four principal lifecycle methods often listed in textbook diagrams.

### Traditional applet example

```java
import java.applet.Applet;
import java.awt.Graphics;

public class HelloApplet extends Applet {
    public void init() {
        System.out.println("Initialized");
    }

    public void start() {
        System.out.println("Started");
    }

    public void paint(Graphics g) {
        g.drawString("Hello Java", 20, 20);
    }

    public void stop() {
        System.out.println("Stopped");
    }

    public void destroy() {
        System.out.println("Destroyed");
    }
}
```

This example is for historical syllabus understanding. It is not intended to run in a modern browser.

## 11. Applet vs Application

| Applet (historical) | Standalone application |
|---|---|
| Traditionally embedded in an applet-enabled environment | Runs as a standalone program |
| Used lifecycle callbacks such as `init()` and `start()` | Commonly starts at `main()` |
| Historically subject to a browser security sandbox | Runs under the permissions granted to the process |
| Required an applet-capable runtime environment | Does not require a browser applet plug-in |

**Exam trap:** An applet traditionally did not use `main()` as its entry point. Its lifecycle was managed by the applet environment.

## 12. Security Considerations

Network programs must validate input and handle failures carefully.

Important concepts:
- Do not trust data received from clients.
- Use appropriate timeouts so operations do not block indefinitely.
- Close sockets and streams.
- Handle `IOException` and related exceptions.
- Use encryption and authentication where appropriate.
- Avoid exposing sensitive information in error messages.

Old applet sandbox rules are historical. Modern Java application security should be understood in the context of current platform and deployment practices.

## 13. Rapid Revision Table

| Topic | Key fact |
|---|---|
| Networking package | `java.net` |
| TCP client | `Socket` |
| TCP server | `ServerSocket` |
| TCP connection acceptance | `accept()` |
| UDP socket | `DatagramSocket` |
| UDP data unit | `DatagramPacket` |
| Host resolution | `InetAddress` |
| URL resource identifier | `URL` |
| TCP behavior | Reliable, ordered byte stream |
| UDP behavior | Datagrams without delivery guarantees |
| Applet initialization | `init()` |
| Applet activation | `start()` |
| Applet deactivation | `stop()` |
| Applet unloading | `destroy()` |
