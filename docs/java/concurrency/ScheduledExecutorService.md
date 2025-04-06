---
sidebar_position: 14
title: ScheduledExecutorService
description: ScheduledExecutorService
keywords: [java, concurrency, ScheduledExecutorService]
---

# ScheduledExecutorService

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
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class ScheduleExecutorService {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(3);
        for (int i = 0; i < 5; i++) {
            Task task = new Task("Task " + i);
            scheduledExecutorService.schedule(task, i * 2, TimeUnit.SECONDS);
        }
        scheduledExecutorService.scheduleAtFixedRate(new Task("Fixed Rate Task"), 1, 5, TimeUnit.SECONDS);
        scheduledExecutorService.scheduleWithFixedDelay(new Task("Fixed Delay Task"), 2, 5, TimeUnit.SECONDS);
        try {
            Thread.sleep(20 * 1000);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        scheduledExecutorService.shutdown();
        System.out.println("ScheduledExecutorService shutdown.");
    }
}
```
