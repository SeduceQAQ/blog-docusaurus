---
sidebar_position: 39
title: Unit Testing Multithreaded Code
description: Unit Testing Multithreaded Code
keywords: [java, concurrency, Unit Testing Multithreaded Code]
---

# Unit Testing Multithreaded Code

```java
import java.util.concurrent.locks.ReentrantLock;

public class ThreadSafeCounter {
    private final ReentrantLock lock = new ReentrantLock();
    private int counter = 0;

    public void increment() {
        lock.lock();
        counter++;
        lock.unlock();
    }

    public int getCounter() {
        lock.lock();
        try {
            return counter;
        } finally {
            lock.unlock();
        }
    }
}
```

```java
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ThreadSafeCounterTest {
    @Test
    public void testCounterWithMultipleThreads() throws InterruptedException {
        ThreadSafeCounter counter = new ThreadSafeCounter();
        ExecutorService executorService = Executors.newFixedThreadPool(10);

        for (int i = 0; i < 1000; i++) {
            executorService.submit(counter::increment);
        }

        executorService.shutdown();

        executorService.awaitTermination(1, TimeUnit.SECONDS);

        Assertions.assertEquals(1000, counter.getCounter(), "Counter value is incorrect!!!");
    }
}
```
