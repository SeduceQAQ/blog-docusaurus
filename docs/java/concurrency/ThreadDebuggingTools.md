---
sidebar_position: 38
title: Thread Debugging Tools
description: Thread Debugging Tools
keywords: [java, concurrency, Thread Debugging Tools]
---

# Thread Debugging Tools

## jps

```java
import java.util.concurrent.locks.ReentrantLock;

public class DeadlockExample {
    private static final ReentrantLock lock1 = new ReentrantLock();
    private static final ReentrantLock lock2 = new ReentrantLock();

    public static void main(String[] args) {
        Runnable task1 = () -> {
            try {
                lock1.lock();
                System.out.println("Thread 1 acquired lock 1");
                Thread.sleep(500);
                lock2.lock();
                System.out.println("Thread 2 acquired lock 2");
                Thread.sleep(500);
                lock2.unlock();
                Thread.sleep(500);
                lock1.unlock();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        };

        Runnable task2 = () -> {
            try {
                lock2.lock();
                System.out.println("Thread 2 acquired lock 2");
                Thread.sleep(500);
                lock1.lock();
                System.out.println("Thread 1 acquired lock 1");
                Thread.sleep(500);
                lock1.unlock();
                Thread.sleep(500);
                lock2.unlock();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        };

        Thread t1 = new Thread(task1, "Thread 1");
        Thread t2 = new Thread(task2, "Thread 2");

        t1.start();
        t2.start();
    }
}
```

```powershell
jps
jstack <pid>
```
