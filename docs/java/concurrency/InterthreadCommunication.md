---
sidebar_position: 19
title: InterthreadCommunication
description: InterthreadCommunication
keywords: [java, concurrency, InterthreadCommunication]
---

# Interthread Communication

```java
public class SharedResource {
    private int data;
    private boolean hasData = false;

    // producer
    public synchronized void produce(int value) {
        while (hasData) {
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        data = value;
        System.out.println(Thread.currentThread().getName() + " produced: " + value);
        hasData = true;
        notify();
    }

    // consume
    public synchronized void consume() {
        while (!hasData) {
            try {
                wait();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        System.out.println(Thread.currentThread().getName() + " consumed: " + data);
        hasData = false;
        notify();
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        SharedResource sharedResource = new SharedResource();

        // producer
        Thread producerThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                sharedResource.produce(i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }, "Producer Thread");

        // consumer
        Thread consumerThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                sharedResource.consume();
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "Consumer Thread");

        producerThread.start();
        consumerThread.start();
    }
}
```
