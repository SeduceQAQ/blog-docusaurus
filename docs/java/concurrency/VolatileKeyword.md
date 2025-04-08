---
sidebar_position: 11
title: Volatile Keyword
description: Volatile Keyword
keywords: [java, concurrency, Volatile Keyword]
---

# Volatile Keyword

```java
public class Counter {
    private volatile boolean running = true;

    public void stopRunning() {
        running = false;
    }

    public void run() {
        while (running) {
            System.out.println("Thread is running...");
        }
        System.out.println("Thread is stopped.");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread thread = new Thread(counter::run);
        thread.start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        counter.stopRunning();
    }
}
```
