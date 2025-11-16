# Day 2 - Java Concurrency - Threads & Runnables

**Date**: November 17, 2025
**Category**: Java Core
**Difficulty**: Intermediate
**Time Spent**: 60 minutes

---

## 📚 What is Concurrency?

**Concurrency** is the ability to execute multiple tasks or processes simultaneously (or appear to do so). In Java, this is achieved primarily through **threads**.

**Thread**: A lightweight process - the smallest unit of execution that can be scheduled by the operating system.

**Simple analogy**: If your program is a restaurant, threads are the waiters. Multiple waiters (threads) can serve different tables (tasks) simultaneously, making the restaurant (program) more efficient.

---

## 🎯 Why is it Important?

- **Real-world use case 1**: **Web servers** - Handle thousands of concurrent user requests (each request on a separate thread)
- **Real-world use case 2**: **Your PMC system** - BackupThreadPoolService manages multiple backup operations concurrently
- **Real-world use case 3**: **Video streaming** - One thread downloads data, another decodes video, another handles audio

- **Interview relevance**: Concurrency questions are COMMON in senior interviews. Interviewers want to know:
  - Do you understand thread safety?
  - Can you prevent race conditions?
  - Do you know when to use concurrency?

---

## 🔍 Deep Dive

### Creating Threads in Java (3 Ways)

#### 1. Extending Thread Class

```java
// Method 1: Extend Thread class
public class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

// Usage
MyThread thread = new MyThread();
thread.start();  // Starts a new thread
```

**Pros**: Simple, straightforward
**Cons**: Can't extend another class (Java single inheritance)

---

#### 2. Implementing Runnable Interface ✓ (Preferred)

```java
// Method 2: Implement Runnable (BETTER)
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable running: " + Thread.currentThread().getName());
    }
}

// Usage
Thread thread = new Thread(new MyRunnable());
thread.start();

// Or with lambda (Java 8+)
Thread thread2 = new Thread(() -> {
    System.out.println("Lambda thread: " + Thread.currentThread().getName());
});
thread2.start();
```

**Pros**:
- Can implement other interfaces
- Better OOP design (composition over inheritance)
- Cleaner with lambdas

**Cons**: Slightly more verbose than extending Thread

---

#### 3. Using Executor Framework ✓✓ (Production Standard)

```java
// Method 3: Executor Service (BEST for production)
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

ExecutorService executor = Executors.newFixedThreadPool(5);

// Submit tasks
executor.submit(() -> {
    System.out.println("Task 1: " + Thread.currentThread().getName());
});

executor.submit(() -> {
    System.out.println("Task 2: " + Thread.currentThread().getName());
});

// Shutdown executor when done
executor.shutdown();
```

**Pros**:
- Thread pooling (reuses threads, efficient)
- Better resource management
- Production-ready
- **This is what you use in PMC BackupThreadPoolService!**

**Cons**: Slightly more complex for simple cases

---

### Thread Lifecycle

```
NEW → RUNNABLE → RUNNING → TERMINATED
         ↓          ↓
      BLOCKED   WAITING
```

**States:**
1. **NEW** - Thread created but not started
2. **RUNNABLE** - Thread is ready to run (waiting for CPU)
3. **RUNNING** - Thread is executing
4. **BLOCKED** - Thread waiting for a lock
5. **WAITING** - Thread waiting for another thread (join, wait)
6. **TERMINATED** - Thread finished execution

---

## 💻 Code Implementation

### Example 1: Basic Thread Creation

```java
public class ThreadBasics {
    public static void main(String[] args) {
        // Method 1: Extend Thread
        Thread thread1 = new MyThread();

        // Method 2: Runnable with anonymous class
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println("Thread2: " + i);
                    try {
                        Thread.sleep(100);  // Sleep 100ms
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });

        // Method 3: Lambda (Java 8+)
        Thread thread3 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread3: " + i);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // Start all threads
        thread1.start();
        thread2.start();
        thread3.start();

        System.out.println("Main thread continues...");
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Thread1: " + i);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**Output** (order not guaranteed!):
```
Main thread continues...
Thread1: 0
Thread2: 0
Thread3: 0
Thread1: 1
Thread3: 1
Thread2: 1
...
```

**Key observation:** Threads run concurrently, output is interleaved!

---

### Example 2: Thread Synchronization Problem (Race Condition)

```java
// ❌ PROBLEM: Race condition (not thread-safe)
public class Counter {
    private int count = 0;

    public void increment() {
        count++;  // NOT atomic! (read, increment, write)
    }

    public int getCount() {
        return count;
    }
}

// Test with multiple threads
public class RaceConditionDemo {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        // Create 1000 threads, each incrementing 1000 times
        Thread[] threads = new Thread[1000];
        for (int i = 0; i < 1000; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    counter.increment();
                }
            });
            threads[i].start();
        }

        // Wait for all threads to finish
        for (Thread thread : threads) {
            thread.join();
        }

        System.out.println("Final count: " + counter.getCount());
        // Expected: 1,000,000
        // Actual: Less than 1,000,000 (race condition!)
    }
}
```

**Problem:** Multiple threads read-modify-write the same variable → lost updates!

---

### Example 3: Thread Synchronization Solution

```java
// ✓ SOLUTION 1: Synchronized method
public class SynchronizedCounter {
    private int count = 0;

    public synchronized void increment() {
        count++;  // Only one thread at a time
    }

    public synchronized int getCount() {
        return count;
    }
}

// ✓ SOLUTION 2: Synchronized block (finer control)
public class SynchronizedBlockCounter {
    private int count = 0;
    private final Object lock = new Object();

    public void increment() {
        synchronized (lock) {
            count++;
        }
    }

    public int getCount() {
        synchronized (lock) {
            return count;
        }
    }
}

// ✓ SOLUTION 3: AtomicInteger (best for simple counters)
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();  // Atomic operation
    }

    public int getCount() {
        return count.get();
    }
}
```

**Now the output will be exactly 1,000,000!**

---

### Example 4: Real-World - Producer-Consumer Pattern

```java
import java.util.LinkedList;
import java.util.Queue;

public class ProducerConsumerExample {
    private static final int CAPACITY = 5;
    private final Queue<Integer> queue = new LinkedList<>();

    // Producer thread
    public void produce() throws InterruptedException {
        int value = 0;
        while (true) {
            synchronized (queue) {
                // Wait if queue is full
                while (queue.size() == CAPACITY) {
                    queue.wait();
                }

                System.out.println("Produced: " + value);
                queue.add(value++);

                // Notify consumer
                queue.notifyAll();

                Thread.sleep(100);
            }
        }
    }

    // Consumer thread
    public void consume() throws InterruptedException {
        while (true) {
            synchronized (queue) {
                // Wait if queue is empty
                while (queue.isEmpty()) {
                    queue.wait();
                }

                int value = queue.poll();
                System.out.println("Consumed: " + value);

                // Notify producer
                queue.notifyAll();

                Thread.sleep(200);
            }
        }
    }

    public static void main(String[] args) {
        ProducerConsumerExample pc = new ProducerConsumerExample();

        Thread producer = new Thread(() -> {
            try {
                pc.produce();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        Thread consumer = new Thread(() -> {
            try {
                pc.consume();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        producer.start();
        consumer.start();
    }
}
```

**This is similar to your PMC message queue pattern with ActiveMQ!**

---

## 🏢 Real-World Examples

### Company 1: Your PMC System (BackupThreadPoolService)

**What you do:**
```java
// Your PMC code (simplified)
ExecutorService realtimeExecutor = Executors.newFixedThreadPool(10);
ExecutorService scanExecutor = Executors.newFixedThreadPool(5);

// Submit backup tasks
realtimeExecutor.submit(new BackupCommand());
scanExecutor.submit(new ScanCommand());
```

**Concurrency patterns you already use:**
- Thread pools for different priority levels
- BlockingQueues for task queuing
- Semaphores for resource throttling
- **You're already doing senior-level concurrency!**

---

### Company 2: Tomcat Web Server

**Challenge**: Handle thousands of concurrent HTTP requests

**How they do it:**
```java
// Tomcat's thread pool (simplified)
ExecutorService executor = new ThreadPoolExecutor(
    10,      // core pool size
    200,     // max pool size
    60,      // keepalive time
    TimeUnit.SECONDS,
    new LinkedBlockingQueue<>(100)  // task queue
);

// Each HTTP request handled by a thread
executor.submit(() -> handleRequest(request));
```

---

### Company 3: Kafka Message Broker

**Challenge**: Process millions of messages per second

**How they do it:**
- Producer threads write to partitions
- Consumer threads read from partitions
- Multiple threads per partition for parallelism
- Thread-safe data structures everywhere

---

## 🎤 Common Interview Questions

### Question 1:
**Q**: What's the difference between `start()` and `run()` in Thread?

**A**:
- **`start()`**: Creates a NEW thread and calls run() in that thread (concurrent execution)
- **`run()`**: Just calls the method in the CURRENT thread (sequential execution, no concurrency!)

```java
Thread t = new Thread(() -> System.out.println("Hello"));
t.start();  // ✓ Creates new thread
t.run();    // ❌ Runs in current thread (no concurrency!)
```

---

### Question 2:
**Q**: How do you prevent race conditions in Java?

**A**: Multiple ways:
1. **Synchronized methods/blocks** - Only one thread accesses at a time
2. **Atomic classes** - AtomicInteger, AtomicLong for lock-free operations
3. **Concurrent collections** - ConcurrentHashMap, CopyOnWriteArrayList
4. **Immutable objects** - Cannot be modified, inherently thread-safe
5. **Thread-local variables** - Each thread has its own copy

**In my PMC work, I use semaphores and concurrent data structures to prevent race conditions in the backup system.**

---

### Question 3:
**Q**: What's the difference between synchronized and ReentrantLock?

**A**:

| Feature | synchronized | ReentrantLock |
|---------|-------------|---------------|
| Usage | Keyword (built-in) | Class (java.util.concurrent) |
| Try lock | No | Yes (tryLock()) |
| Timeout | No | Yes (tryLock(timeout)) |
| Fairness | No | Yes (fair mode) |
| Interruptible | No | Yes |
| Performance | Good | Slightly better |

**When to use:**
- **synchronized**: Simple cases, easier to read
- **ReentrantLock**: Need tryLock, timeout, or fairness

---

### Question 4:
**Q**: Explain thread pooling and why it's better than creating new threads.

**A**:
**Thread pooling** reuses a fixed set of threads instead of creating new ones for each task.

**Advantages:**
1. **Performance** - Creating threads is expensive (OS resources)
2. **Resource control** - Limit max concurrent threads
3. **Queue management** - Built-in task queuing
4. **Lifecycle management** - Automatic thread cleanup

```java
// ❌ BAD: Create thread for each task (expensive!)
for (int i = 0; i < 10000; i++) {
    new Thread(task).start();  // 10,000 threads!
}

// ✓ GOOD: Use thread pool (reuse 10 threads)
ExecutorService executor = Executors.newFixedThreadPool(10);
for (int i = 0; i < 10000; i++) {
    executor.submit(task);  // Only 10 threads, reused!
}
```

**In PMC, we use separate thread pools for realtime vs scan operations to prioritize critical tasks.**

---

## 🔗 Related Concepts

- **Thread Safety** - Ensuring code works correctly with multiple threads
- **Deadlock** - Two threads waiting for each other (circular dependency)
- **Livelock** - Threads actively responding but making no progress
- **Starvation** - Thread never gets CPU time
- **Volatile keyword** - Ensures visibility of changes across threads
- **ExecutorService** - High-level thread pool management (tomorrow's topic!)

---

## 📝 Hands-On Exercise

**Task**: Implement a thread-safe BankAccount class

**Requirements:**
```java
public class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    // TODO: Make this thread-safe
    public void deposit(double amount) {
        balance += amount;
    }

    // TODO: Make this thread-safe
    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }

    // TODO: Make this thread-safe
    public double getBalance() {
        return balance;
    }
}

// Test: 10 threads depositing $100, 10 threads withdrawing $50
// Final balance should be: initial + (10*100) - (10*50) = initial + 500
```

**Solution**:
```java
public class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public synchronized void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount + ", Balance: " + balance);
    }

    public synchronized void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println("Withdrew: " + amount + ", Balance: " + balance);
        } else {
            System.out.println("Insufficient funds for withdrawal: " + amount);
        }
    }

    public synchronized double getBalance() {
        return balance;
    }
}
```

**What I learned from this exercise**:
The `synchronized` keyword ensures only one thread can access the method at a time, preventing race conditions in read-modify-write operations. This is critical for financial transactions!

---

## 📖 Resources & References

- **Book**: "Java Concurrency in Practice" by Brian Goetz (THE definitive guide)
- **Oracle Docs**: https://docs.oracle.com/javase/tutorial/essential/concurrency/
- **Your PMC Code**: BackupThreadPoolService.java - real production example!
- **Article**: "A Guide to Java ExecutorService" - Baeldung

---

## ✅ Concept Mastery Checklist

- [ ] Can create threads using all 3 methods
- [ ] Understand thread lifecycle
- [ ] Can identify race conditions
- [ ] Know how to use synchronized
- [ ] Understand thread pools
- [ ] Can answer interview questions confidently
- [ ] Completed hands-on exercise

---

## 🧠 Key Takeaways (In Your Own Words)

1. **Threads enable concurrency** - Multiple tasks running simultaneously for better performance
2. **Runnable > Extend Thread** - Better OOP, can implement other interfaces
3. **Thread Pools are production standard** - Reuse threads, manage resources efficiently
4. **Race conditions are real** - Always think about thread safety when sharing data
5. **synchronized is your friend** - Simple way to prevent race conditions, but use wisely (can impact performance)
6. **I already use this in PMC!** - BackupThreadPoolService, priority queues, concurrent collections

---

## 📅 Review Schedule

- [ ] Review after 1 day (Nov 18)
- [ ] Review after 1 week (Nov 24)
- [ ] Review after 1 month (Dec 17)

---

## 💭 Personal Notes

**Connection to PMC work:**
My BackupThreadPoolService uses ExecutorService with multiple thread pools. I manage:
- Priority queues (realtime vs scan)
- Semaphores for resource throttling
- BlockingQueues for task scheduling
- This is EXACTLY what senior engineers do!

**For interviews, I can talk about:**
- "I designed a multi-priority thread pool system for backup operations"
- "I used semaphores to prevent resource exhaustion"
- "I implemented fair scheduling across multiple queues"

**Next step:** Tomorrow (Day 3) will cover **System Design - Load Balancing**, which ties into distributing work across threads/servers!
