---
sidebar_position: 32
title: Concurrency Design Patterns
description: Concurrency Design Patterns
keywords: [java, concurrency, Concurrency Design Patterns]
---

# Concurrency Design Patterns

## Producer-Consumer Pattern

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.TimeUnit;

public class Producer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        try {
            for (int i = 0; i < 10; i++) {
                int value = (int) (Math.random() * 100);
                System.out.println("Produced: " + value);
                queue.put(value);
                TimeUnit.MILLISECONDS.sleep(500);
            }
            queue.put(-1);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.TimeUnit;

public class Consumer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        try {
            while (true) {
                Integer value = queue.take();
                if (value == -1) {
                    break;
                }
                System.out.println("Consumed: " + value);
                TimeUnit.MILLISECONDS.sleep(500);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class ProducerConsumerPatternExample {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);
        Thread producer = new Thread(new Producer(queue));
        Thread consumer = new Thread(new Consumer(queue));

        producer.start();
        consumer.start();

        try {
            producer.join();
            consumer.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

## Thread Per Message Pattern

```java
public class PrintRequestHandler implements Runnable {
    private final String message;

    public PrintRequestHandler(String message) {
        this.message = message;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " processing: " + message);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println(Thread.currentThread().getName() + "  completed: " + message);
    }
}
```

```java
public class ThreadPerMessageExample {
    public static void main(String[] args) {
        String[] messages = {"Message 1", "Message 2", "Message 3", "Message 4", "Message 5"};
        for (String message : messages) {
            Thread thread = new Thread((new PrintRequestHandler(message)));
            thread.start();
        }
    }
}
```

## Worker Thread Pattern

```java
public class Task implements Runnable {
    private final int taskId;

    public Task(int taskId) {
        this.taskId = taskId;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " processing task: " + taskId);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println(Thread.currentThread().getName() + " completed task: " + taskId);
    }
}
```

```java
import java.util.concurrent.BlockingQueue;

public class Worker extends Thread {
    private final BlockingQueue<Runnable> taskQueue;

    public Worker(BlockingQueue<Runnable> taskQueue) {
        this.taskQueue = taskQueue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                Runnable task = taskQueue.take();
                task.run();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
}
```

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class WorkerThreadPatternExample {
    public static void main(String[] args) {
        BlockingQueue<Runnable> taskQueue = new LinkedBlockingQueue<>();
        int numberOfWorkers = 3;
        Worker[] workers = new Worker[numberOfWorkers];

        // Start workers
        for (int i = 0; i < numberOfWorkers; i++) {
            workers[i] = new Worker(taskQueue);
            workers[i].start();
        }

        // Add tasks to the queue
        for (int i = 0; i < 10; i++) {
            taskQueue.add(new Task(i));
        }

        // Stop workers after all tasks are processed
        try {
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        // Interrupt workers to stop them
        for (Worker worker : workers) {
            worker.interrupt();
        }
    }
}
```
