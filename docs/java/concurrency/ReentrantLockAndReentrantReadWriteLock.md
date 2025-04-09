---
sidebar_position: 21
title: ReentrantLock and ReentrantReadWriteLock
description: ReentrantLock and ReentrantReadWriteLock
keywords: [java, concurrency, ReentrantLock and ReentrantReadWriteLock]
---

# ReentrantLock and ReentrantReadWriteLock

## ReentrantLock

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantLockExample {
    private final Lock lock = new ReentrantLock();
    private int counter = 0;

    public void increment() {
        lock.lock();
        try {
            counter++;
            System.out.println(Thread.currentThread().getName() + " Counter: " + counter);
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        ReentrantLockExample reentrantLockExample = new ReentrantLockExample();

        Thread thread1 = new Thread(reentrantLockExample::increment, "Thread 1");
        Thread thread2 = new Thread(reentrantLockExample::increment, "Thread 2");

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();
    }
}
```

## ReentrantReadWriteLock

```java
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockExample {
    private final ReentrantReadWriteLock lock = new ReentrantReadWriteLock();
    private final ReentrantReadWriteLock.ReadLock readLock = lock.readLock();
    private final ReentrantReadWriteLock.WriteLock writeLock = lock.writeLock();
    private int data = 0;

    // write method
    public void writeData(int value) {
        writeLock.lock();
        try {
            data = value;
            System.out.println(Thread.currentThread().getName() + " wrote: " + data);
        } finally {
            writeLock.unlock();
        }
    }

    // read method
    public void readData() {
        readLock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + " read: " + data);
        } finally {
            readLock.unlock();
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        ReadWriteLockExample readWriteLockExample = new ReadWriteLockExample();
        Thread writerThread = new Thread(() -> readWriteLockExample.writeData(1), "Writer Thread");
        Thread readerThread1 = new Thread(readWriteLockExample::readData, "Reader Thread 1");
        Thread readerThread2 = new Thread(readWriteLockExample::readData, "Reader Thread 2");

        writerThread.start();
        readerThread1.start();
        readerThread2.start();

        writerThread.join();
        readerThread1.join();
        readerThread2.join();
    }
}
```
