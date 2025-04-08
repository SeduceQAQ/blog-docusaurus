---
sidebar_position: 17
title: AtomicVariables
description: AtomicVariables
keywords: [java, concurrency, AtomicVariables]
---

# AtomicVariables

```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        AtomicInteger atomicInteger = new AtomicInteger(0);
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        for (int i = 0; i < 5; i++) {
            executorService.submit(() -> {
                String threadName = Thread.currentThread().getName();
                int newValueOfCounter = atomicInteger.incrementAndGet();
                System.out.println(threadName + " incremented the value of counter to " + newValueOfCounter);
            });
        }
        executorService.shutdown();
        executorService.awaitTermination(10, TimeUnit.SECONDS);
        System.out.println("Final value of counter: " + atomicInteger.get());
    }
}
```
