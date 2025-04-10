---
sidebar_position: 29
title: JVMs Role in Thread Management
description: JVMs Role in Thread Management
keywords: [java, concurrency, JVMs Role in Thread Management]
---

# JVMs Role in Thread Management

```java
public class CustomTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread Name: " + Thread.currentThread().getName() + ", State: " + Thread.currentThread().getState());
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new CustomTask(), "Thread-1");
        Thread t2 = new Thread(new CustomTask(), "Thread-2");
        Thread t3 = new Thread(new CustomTask(), "Thread-3");

        System.out.println("Thread-1 State Before Start: " + t1.getState());
        System.out.println("Thread-2 State Before Start: " + t2.getState());
        System.out.println("Thread-3 State Before Start: " + t3.getState());

        t1.start();
        t2.start();
        t3.start();

        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        System.out.println("Thread-1 State After Start: " + t1.getState());
        System.out.println("Thread-2 State After Start: " + t2.getState());
        System.out.println("Thread-3 State After Start: " + t3.getState());
    }
}
```
