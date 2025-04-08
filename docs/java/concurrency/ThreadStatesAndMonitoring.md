---
sidebar_position: 8
title: Thread States and Monitoring
description: Thread States and Monitoring
keywords: [java, concurrency, Thread States and Monitoring]
---

# Thread States and Monitoring

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running...");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        System.out.println("Before start: " + myThread.getState()); // NEW
        myThread.start();
        System.out.println("After start: " + myThread.getState()); // RUNNABLE
    }
}
```
