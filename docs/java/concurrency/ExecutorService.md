---
sidebar_position: 13
title: ExecutorService
description: ExecutorService
keywords: [java, concurrency, ExecutorService]
---

# ExecutorService

```java
public class Task implements Runnable {

    private String taskName;

    public Task(String taskName) {
        this.taskName = taskName;
    }

    @Override
    public void run() {
        System.out.println("Executing task: " + taskName + " by thread: " + Thread.currentThread().getName());
        try {
            Thread.sleep(2000L);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println(taskName + " is completed.");
    }
}
```

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        for (int i = 0; i < 6; i++) {
            Task task = new Task("Task " + i);
            executorService.submit(task);
        }
        executorService.shutdown();
        System.out.println("All tasks submitted.");

        try {
            if (!executorService.awaitTermination(60, TimeUnit.SECONDS)) {
                executorService.shutdownNow();
            }
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        System.out.println("Executor service shut down.");
    }
}
```

## Thread Pool

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService singleThreadPool = Executors.newSingleThreadExecutor();
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
        ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();
        ExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(3);
        ExecutorService newSingleThreadScheduledThreadPool = Executors.newSingleThreadScheduledExecutor();
    }
}
```
