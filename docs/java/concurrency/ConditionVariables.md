---
sidebar_position: 22
title: Condition Variables
description: Condition Variables
keywords: [java, concurrency, Condition Variables]
---

# Condition Variables

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class SharedQueue {
    private final int capacity = 5;
    private final ReentrantLock lock = new ReentrantLock();
    private final Condition notEmpty = lock.newCondition();
    private final Condition notFull = lock.newCondition();
    Queue<Integer> queue = new LinkedList<>();

    public void produce(int value) throws InterruptedException {
        lock.lock();
        try {
            while (queue.size() == capacity) {
                System.out.println("Queue is full. Producer is waiting...");
                notFull.await(); // wait until the queue is not full
            }
            queue.add(value); // add the item to the queue
            System.out.println("Produced: " + value);
            notEmpty.signal(); // notify the consumer that the queue is not empty
        } finally {
            lock.unlock();
        }
    }

    public void consume() throws InterruptedException {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                System.out.println("Queue is empty. Consumer is waiting...");
                notEmpty.await(); // wait until the queue is not empty
            }
            int value = queue.poll(); // remove an item from the queue
            System.out.println("Consumed: " + value);
            notFull.signal(); // notify the producer that the queue is not full
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        SharedQueue sharedQueue = new SharedQueue();

        Thread producer = new Thread(() -> {
            for (int i = 0; i < 11; i++) {
                try {
                    sharedQueue.produce(i);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        Thread consumer = new Thread(() -> {
            for (int i = 0; i < 11; i++) {
                try {
                    sharedQueue.consume();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        producer.start();
        consumer.start();
    }
}
```
