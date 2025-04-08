---
sidebar_position: 5
title: Create Runnable Class
description: Create Runnable Class
keywords: [java, concurrency, Create Runnable Class]
---

# Create Runnable Class

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Creating thread with Runnable: Thread is running...");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread myThread = new Thread(myRunnable);
        myThread.start();
        System.out.println("Thread is started.");
    }
}
```
