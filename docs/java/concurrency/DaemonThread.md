---
sidebar_position: 31
title: Daemon Thread
description: Daemon Thread
keywords: [java, concurrency, Daemon Thread]
---

# Daemon Thread

```java
public class Main {
    public static void main(String[] args) {
        Thread userThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("User thread: " + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "User Thread");

        Thread daemonThread = new Thread(() -> {
            while (true) {
                System.out.println("Daemon thread is running...");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "Daemon Thread");

        daemonThread.setDaemon(true);

        userThread.start();
        daemonThread.start();
    }
}
```
