---
sidebar_position: 26
title: Thread Safety and Immutability
description: Thread Safety and Immutability
keywords: [java, concurrency, Thread Safety and Immutability]
---

# Thread Safety and Immutability

## Thread Safety

```java
public class ThreadSafeCounter {
    private int counter = 0;

    public synchronized void increment() {
        counter++;
        System.out.println(Thread.currentThread().getName() + " incremented the counter to " + counter);
    }

    public int getCounter() {
        return counter;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        ThreadSafeCounter counter = new ThreadSafeCounter();
        Runnable task = counter::increment;

        Thread t1 = new Thread(task, "Thread 1");
        Thread t2 = new Thread(task, "Thread 2");
        Thread t3 = new Thread(task, "Thread 3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```

## Immutability

```java
public class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Person person = new Person("Alice", 30);

        Runnable task = () -> System.out.println(Thread.currentThread().getName() + ": " + person.getName() + " is " + person.getAge() + " years old");

        Thread t1 = new Thread(task, "Thread 1");
        Thread t2 = new Thread(task, "Thread 2");
        Thread t3 = new Thread(task, "Thread 3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```
