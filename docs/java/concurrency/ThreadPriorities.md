---
sidebar_position: 30
title: Thread Priorities
description: Thread Priorities
keywords: [java, concurrency, Thread Priorities]
---

# Thread Priorities

```java
public class Main {
    public static void main(String[] args) {
        Thread highPriorityThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("High-priority thread: " + i);
            }
        }, "High Priority thread");
        Thread mediumPriorityThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Medium-priority thread: " + i);
            }
        }, "Medium Priority thread");
        Thread lowPriorityThread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Low-priority thread: " + i);
            }
        }, "Low Priority thread");

        highPriorityThread.setPriority(Thread.MAX_PRIORITY);
        mediumPriorityThread.setPriority(Thread.NORM_PRIORITY);
        lowPriorityThread.setPriority(Thread.MIN_PRIORITY);

        highPriorityThread.start();
        mediumPriorityThread.start();
        lowPriorityThread.start();
    }
}
```
