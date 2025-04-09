---
sidebar_position: 20
title: Locks and Conditions
description: Locks and Conditions
keywords: [java, concurrency, Locks and Conditions]
---

# Locks and Conditions

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class SharedResourceWithLock {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private int data;
    private boolean hasData = false;

    // producer
    public void produce(int value) {
        lock.lock();
        try {
            while (hasData) {
                condition.await();
            }
            data = value;
            System.out.println(Thread.currentThread().getName() + " produced: " + value);
            hasData = true;
            condition.signal();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            lock.unlock();
        }
    }

    // consume
    public void consume() {
        lock.lock();
        try {
            while (!hasData) {
                condition.await();
            }
            System.out.println(Thread.currentThread().getName() + " consumed: " + data);
            hasData = false;
            condition.signal();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        SharedResourceWithLock sharedResourceWithLock = new SharedResourceWithLock();

        // producer
        Thread producerThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                sharedResourceWithLock.produce(i);
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
                sharedResourceWithLock.consume();
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
