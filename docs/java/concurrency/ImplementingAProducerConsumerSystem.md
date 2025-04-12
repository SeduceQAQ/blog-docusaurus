---
sidebar_position: 36
title: Implementing a Producer-Consumer System
description: Implementing a Producer-Consumer System
keywords: [java, concurrency, Implementing a Producer-Consumer System]
---

# Implementing a Producer-Consumer System

```java
public record Order(int orderId, String productName, int quantity) {
}
```

```java
import java.util.LinkedList;
import java.util.List;

public class SharedBuffer {
    private final List<Order> buffer = new LinkedList<>();
    private final int capacity;

    public SharedBuffer(int capacity) {
        this.capacity = capacity;
    }

    public synchronized void produce(Order order) throws InterruptedException {
        while (buffer.size() == capacity) {
            System.out.println("Buffer is full. Producer is waiting...");
            wait(); // wait until space becomes available
        }
        buffer.add(order);
        System.out.println("New Order Added: " + order);
        notifyAll(); // notify the consumer
    }

    public synchronized Order consume() throws InterruptedException {
        while (buffer.isEmpty()) {
            System.out.println("Buffer is empty. Consumer is waiting...");
            wait(); // wait until an order is available
        }
        Order order = buffer.removeFirst();
        System.out.println("Processing order: " + order);
        notifyAll(); // notify the producer
        return order;
    }
}
```

```java
import java.util.Random;

public class Producer implements Runnable {
    private final SharedBuffer sharedBuffer;
    private int orderId = 0;

    public Producer(SharedBuffer sharedBuffer) {
        this.sharedBuffer = sharedBuffer;
    }

    @Override
    public void run() {
        String[] products = {"Laptop", "Phone", "Headphone", "Monitor", "Keyboard"};
        Random random = new Random();

        while (true) {
            try {
                // Generate a random order
                String product = products[random.nextInt(products.length)];
                int quantity = random.nextInt(1, 10);
                Order order = new Order(++orderId, product, quantity);
                sharedBuffer.produce(order);
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
}
```

```java
public class Consumer implements Runnable {
    private final SharedBuffer sharedBuffer;

    public Consumer(SharedBuffer sharedBuffer) {
        this.sharedBuffer = sharedBuffer;
    }

    @Override
    public void run() {
        while (true) {
            try {
                Order order = sharedBuffer.consume();
                System.out.println("Delivered order: " + order);
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        SharedBuffer sharedBuffer = new SharedBuffer(5);

        Thread producerThread = new Thread(new Producer(sharedBuffer));
        Thread consumerThread = new Thread(new Consumer(sharedBuffer));

        producerThread.start();
        consumerThread.start();
    }
}
```
