---
sidebar_position: 33
title: Handling Exceptions in Threads
description: Handling Exceptions in Threads
keywords: [java, concurrency, Handling Exceptions in Threads]
---

# Handling Exceptions in Threads

## Handling Exceptions with Runnable

```java
public class RunnableExceptionExample {
    public static void main(String[] args) {
        Runnable task = () -> {
            while (true) {
                try {
                    System.out.println(Thread.currentThread().getName() + ": Task started.");
                    double value = Math.random();
                    System.out.println("Generated number for thread: " + Thread.currentThread().getName() + " is " + value);
                    if (value > 0.5) {
                        throw new RuntimeException("Simulated error!");
                    }
                    Thread.sleep(1000);
                    System.out.println(Thread.currentThread().getName() + ": Task completed successfully.");
                } catch (Exception e) {
                    System.out.println(Thread.currentThread().getName() + ": Caught exception: " + e.getMessage());
                    break;
                }
            }
        };

        Thread thread1 = new Thread(task, "Thread 1");
        Thread thread2 = new Thread(task, "Thread 2");

        thread1.start();
        thread2.start();
    }
}
```

## Handling Exceptions with Thread.UncaughtExceptionHandler

```java
public class UnCaughtExceptionHandlerExample {
    public static void main(String[] args) {
        Thread.UncaughtExceptionHandler handler = (t, e) -> System.out.println("Thread " + t.getName() + " terminated with an exception: " + e.getMessage());

        Runnable task = () -> {
            while (true) {
                try {
                    System.out.println(Thread.currentThread().getName() + ": Task started.");
                    double value = Math.random();
                    System.out.println("Generated number for thread: " + Thread.currentThread().getName() + " is " + value);
                    if (value > 0.5) {
                        throw new RuntimeException("Simulated error!");
                    }
                    Thread.sleep(1000);
                    System.out.println(Thread.currentThread().getName() + ": Task completed successfully.");
                } catch (InterruptedException e) {
                    System.out.println(Thread.currentThread().getName() + ": Interrupted");
                    Thread.currentThread().interrupt();
                    break;
                }
            }
        };

        Thread thread1 = new Thread(task, "Thread 1");
        thread1.setUncaughtExceptionHandler(handler);

        Thread thread2 = new Thread(task, "Thread 2");
        thread2.setUncaughtExceptionHandler(handler);

        thread1.start();
        thread2.start();
    }
}
```

## Handling Exceptions with Callable and Future

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class CallableExceptionExample {
    public static void main(String[] args) {
        while (true) {
            ExecutorService executorService = Executors.newSingleThreadExecutor();

            Callable<String> task = () -> {
                System.out.println(Thread.currentThread().getName() + ": Task started.");
                double value = Math.random();
                System.out.println("Generated number for thread: " + Thread.currentThread().getName() + " is " + value);
                if (value > 0.5) {
                    throw new RuntimeException("Simulated error!");
                }
                Thread.sleep(1000);
                return (Thread.currentThread().getName() + ": Task completed successfully.");
            };

            Future<String> future = executorService.submit(task);
            try {
                String result = future.get();
                System.out.println("Result:" + result);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                System.out.println("Task interrupted.");
            } catch (ExecutionException e) {
                System.out.println("Task failed with exception: " + e.getCause().getMessage());
                break;
            } finally {
                executorService.shutdown();
            }
        }
    }
}
```
