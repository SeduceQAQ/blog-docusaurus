---
sidebar_position: 15
title: ConcurrentCollection
description: ConcurrentCollection
keywords: [java, concurrency, ConcurrentCollection]
---

# ConcurrentCollection

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
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ConcurrentHashMapExample {
    public static void main(String[] args) throws InterruptedException {
        Map<String, String> taskResult = new ConcurrentHashMap<>();
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        for (int i = 0; i < 6; i++) {
            String taskName = "task" + i;
            executorService.submit(() -> {
                String threadName = Thread.currentThread().getName();
                System.out.println(threadName + " is executing " + taskName);
                try {
                    Thread.sleep(1000L);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
                taskResult.put(taskName, "Completed by: " + threadName);
            });
        }
        executorService.shutdown();
        executorService.awaitTermination(10, TimeUnit.SECONDS);
        System.out.println("Task result: ");
        taskResult.forEach((task, result) -> {
            System.out.println(task + ": " + result);
        });
    }
}
```
