---
sidebar_position: 10
title: Synchronized Keyword
description: Synchronized Keyword
keywords: [java, concurrency, Synchronized Keyword]
---

# Synchronized Keyword

```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
            System.out.println("Final value of count: " + counter.getCount());
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```
