---
sidebar_position: 16
title: Future and Callable
description: Future and Callable
keywords: [java, concurrency, Future and Callable]
---

# Future and Callable

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        Callable<String> callableTask = () -> {
            String threadName = Thread.currentThread().getName();
            System.out.println(threadName + " is executing Callable task.");
            Thread.sleep(2000);
            return "Result from " + threadName;
        };

        Future<String> futureResult = executorService.submit(callableTask);
        String result = futureResult.get();

        System.out.println(result);
        executorService.shutdown();
    }
}
```
