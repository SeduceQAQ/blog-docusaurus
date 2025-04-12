---
sidebar_position: 34
title: Advanced Locking Mechanisms
description: Advanced Locking Mechanisms
keywords: [java, concurrency, Advanced Locking Mechanisms]
---

# ReentrantLock

```java
import java.util.concurrent.locks.StampedLock;

public class BankAccount {
    private final StampedLock stampedLock = new StampedLock();
    private double balance = 0;

    public void deposit(double amount) {
        long stamp = stampedLock.writeLock();
        try {
            System.out.println(Thread.currentThread().getName() + " try to deposit " + amount + " into the bank");
            balance += amount;
            System.out.println(Thread.currentThread().getName() + " new balance is " + balance);
        } finally {
            stampedLock.unlockWrite(stamp);
        }
    }

    public void withdraw(double amount) {
        long stamp = stampedLock.writeLock();
        try {
            if (balance >= amount) {
                balance -= amount;
                System.out.println(Thread.currentThread().getName() + "withdrew: " + amount + ", New balance is " + balance);
            } else {
                System.out.println(Thread.currentThread().getName() + "withdrew: " + amount + ", Insufficient balance");
            }
        } finally {
            stampedLock.unlockWrite(stamp);
        }
    }

    public double getBalanceOptimistic() {
        long stamp = stampedLock.tryOptimisticRead();
        double currentBalance = balance;

        if (!stampedLock.validate(stamp)) {
            stamp = stampedLock.readLock();
            try {
                currentBalance = balance;
            } finally {
                stampedLock.unlockRead(stamp);
            }
        }

        System.out.println(Thread.currentThread().getName() + "getBalanceOptimistic " + currentBalance);
        return currentBalance;
    }

    public double getBalance() {
        long stamp = stampedLock.readLock();
        try {
            return balance;
        } finally {
            stampedLock.unlockRead(stamp);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        BankAccount bankAccount = new BankAccount();

        Runnable depositTask = () -> {
            for (int i = 0; i < 3; i++) {
                bankAccount.deposit(100);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        };

        Runnable withdrawTask = () -> {
            for (int i = 0; i < 3; i++) {
                bankAccount.withdraw(50);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        };

        Runnable readTask = () -> {
            for (int i = 0; i < 3; i++) {
                bankAccount.getBalanceOptimistic();
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        };

        Thread depositThread = new Thread(depositTask, "Depositor");
        Thread withdrawThread = new Thread(withdrawTask, "Withdrawal");
        Thread readThread = new Thread(readTask, "Reader");

        depositThread.start();
        withdrawThread.start();
        readThread.start();

        try {
            depositThread.join();
            withdrawThread.join();
            readThread.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println("Final balance: " + bankAccount.getBalance());
    }
}
```
