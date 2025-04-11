---
sidebar_position: 28
title: Parallel Streams
description: Parallel Streams
keywords: [java, concurrency, Parallel Streams]
---

# Parallel Streams

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        numbers.parallelStream().forEach(number -> {
            System.out.println("Thread: " + Thread.currentThread().getName() + ", Number: " + number + ", Square: " + number * number);
        });
    }
}
```
