---
sidebar_position: 35
title: Building a Multithreaded Web Server
description: Building a Multithreaded Web Server
keywords: [java, concurrency, Building a Multithreaded Web Server]
---

# Building a Multithreaded Web Server

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.Socket;

public class ClientHandler implements Runnable {
    private final Socket clientSocket;

    public ClientHandler(Socket clientSocket) {
        this.clientSocket = clientSocket;
    }

    @Override
    public void run() {
        try (OutputStream outputStream = clientSocket.getOutputStream()) {
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));

            String inputLine;
            while (!(inputLine = bufferedReader.readLine()).isEmpty()) {
                System.out.println(inputLine);
            }

            String httpResponse = "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\nContent-Length: 11\r\n\r\nHello World!";
            outputStream.write(httpResponse.getBytes());
        } catch (IOException e) {
            System.out.println("Client error: " + e.getMessage());
        } finally {
            try {
                clientSocket.close();
            } catch (IOException e) {
                System.out.println("Failed to close client socket: " + e.getMessage());
            }
        }
    }
}
```

```java
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class MultithreadedWebServer {
    private final int port;

    public MultithreadedWebServer(int port) {
        this.port = port;
    }

    public void server() {
        ExecutorService executorService = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());
        try (ServerSocket serverSocket = new ServerSocket(port)) {
            System.out.println("Server is listening on port " + port);

            while (true) {
                Socket clientSocket = serverSocket.accept();
                executorService.submit(new ClientHandler(clientSocket));
            }
        } catch (Exception e) {
            System.out.println("Server error: " + e.getMessage());
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        MultithreadedWebServer multithreadedWebServer = new MultithreadedWebServer(8080);
        Thread serverThread = new Thread(multithreadedWebServer::server);
        serverThread.start();
        try {
            serverThread.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```
