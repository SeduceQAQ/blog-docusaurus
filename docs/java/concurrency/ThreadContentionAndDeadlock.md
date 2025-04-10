---
sidebar_position: 25
title: Thread Contention and Deadlock
description: Thread Contention and Deadlock
keywords: [java, concurrency, Thread Contention and Deadlock]
---

# Thread Contention and Deadlock

## Thread Contention

```java
import java.util.concurrent.locks.ReentrantLock;

public class Main {
    private static final ReentrantLock lock = new ReentrantLock();
    private static int counter = 0;

    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                lock.lock();
                try {
                    System.out.println(Thread.currentThread().getName() + " is incrementing the counter.");
                    counter++;
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    System.out.println(Thread.currentThread().getName() + " is releasing the lock.");
                    lock.unlock();
                }
            }
        };

        Thread thread1 = new Thread(task, "thread1");
        Thread thread2 = new Thread(task, "thread2");
        Thread thread3 = new Thread(task, "thread3");

        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```

## Deadlock

```java
import java.util.concurrent.locks.ReentrantLock;

public class Main {
    private static final ReentrantLock lock1 = new ReentrantLock();
    private static final ReentrantLock lock2 = new ReentrantLock();

    public static void main(String[] args) {
        Runnable task1 = () -> {
            lock1.lock();
            try {
                System.out.println("Thread1 acquired lock1. Waiting for lock2...");
                Thread.sleep(100);
                lock2.lock();
                try {
                    System.out.println("Thread1 acquired lock2.");
                } finally {
                    lock2.unlock();
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                lock1.unlock();
            }
        };

        Runnable task2 = () -> {
            lock2.lock();
            try {
                System.out.println("Thread2 acquired lock2. Waiting for lock1...");
                Thread.sleep(100);
                lock1.lock();
                try {
                    System.out.println("Thread1 acquired lock1.");
                } finally {
                    lock1.unlock();
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                lock2.unlock();
            }
        };

        Thread t1 = new Thread(task1, "Thread1");
        Thread t2 = new Thread(task2, "Thread2");

        t1.start();
        t2.start();
    }
}
```
