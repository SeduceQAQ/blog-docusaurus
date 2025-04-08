---
sidebar_position: 4
title: Create Thread Class
description: Create Thread Class
keywords: [java, concurrency, Create Thread Class]
---

# Create Thread Class

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
        myThread.start();
        System.out.println("Thread is started.");
    }
}
```
