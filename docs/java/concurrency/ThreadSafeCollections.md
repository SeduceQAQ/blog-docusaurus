---
sidebar_position: 23
title: Thread Safe Collections
description: Thread Safe Collections
keywords: [java, concurrency, Thread Safe Collections]
---

# Thread Safe Collections

## CopyOnWriteArrayList

```java
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class Main {
    public static void main(String[] args) {
        List<String> orderList = new CopyOnWriteArrayList<>();

        Thread thread1 = new Thread(() -> addOrder(orderList, "Order 1"));
        Thread thread2 = new Thread(() -> addOrder(orderList, "Order 2"));

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }

        System.out.println("Final order list: " + orderList);
    }

    public static void addOrder(List<String> orderList, String order) {
        orderList.add(order);
        System.out.println(Thread.currentThread().getName() + " added: " + order);
    }
}
```
