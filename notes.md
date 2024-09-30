# Notes : Practical Concurrent and Parallel Programming Autumn 2024 @ ITU

## Lecture 01 : Intro to Concurrency and Mutual Exclusion

### **Introduction to Concurrency**

Concurrency is the simultaneous execution of multiple interacting computational tasks. It is critical in modern computing, as systems often need to handle multiple tasks or processes concurrently. This lecture focuses on foundational concepts of concurrency, Java threads, and mutual exclusion, essential for writing correct and efficient concurrent software.

#### **Why Concurrency?**
1. **Exploitation of Hardware Capabilities**: Modern computers, particularly those with multicore processors, can execute multiple instructions at once. To utilize these processors fully, software must be able to handle parallel tasks.
2. **Responsiveness**: Applications such as user interfaces (UI) or web servers benefit from concurrency to remain responsive while handling multiple tasks.
3. **Shared Resources**: Concurrency enables different processes to share and efficiently use system resources like memory, CPU time, etc.

---

### **Java Threads**

Java provides built-in support for multithreading. A thread is a lightweight process that runs within a program and enables multiple tasks to be performed simultaneously.

#### **Basic Thread Creation in Java**
- **`Thread` Class**: In Java, you can create a thread by subclassing the `Thread` class and overriding its `run()` method.
- **`Runnable` Interface**: Alternatively, you can implement the `Runnable` interface and pass the instance to a `Thread` object. This is more flexible because it allows the class to extend another class while implementing multithreading.

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running.");
    }
}

class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread via Runnable is running.");
    }
}

Thread thread1 = new MyThread();
Thread thread2 = new Thread(new MyRunnable());

thread1.start();
thread2.start();
```

### **Concurrency Challenges**

Concurrency introduces several challenges:
- **Nondeterminism**: The exact order in which threads execute can be unpredictable, leading to different results in different runs of the same program.
- **Race Conditions**: A race condition occurs when the outcome depends on the non-deterministic timing of multiple threads accessing shared data.
- **Thread Interference**: When multiple threads try to modify shared variables at the same time without proper synchronization, it can lead to unexpected behavior.

---

### **Mutual Exclusion**

Mutual exclusion ensures that only one thread can execute a critical section at any given time, preventing race conditions.

#### **Critical Sections**
A critical section is a piece of code that accesses shared resources (like variables or objects) that must not be concurrently accessed by more than one thread. To achieve mutual exclusion in Java:
- **Locks**: Use `synchronized` methods or blocks to ensure only one thread can execute the critical section.

Example:
```java
public synchronized void increment() {
    // Critical section
}
```

Alternatively:
```java
public void increment() {
    synchronized(this) {
        // Critical section
    }
}
```

---

### **Locks in Java**

In Java, you can use higher-level locking mechanisms provided by the `java.util.concurrent.locks` package, which gives more flexibility than the `synchronized` keyword.

- **`ReentrantLock`**: This lock allows a thread to enter the same lock more than once, hence the term "reentrant."
- **Lock Methods**:
  - `lock()`: Acquires the lock.
  - `unlock()`: Releases the lock.

Example:
```java
Lock lock = new ReentrantLock();

lock.lock();
try {
    // Critical section
} finally {
    lock.unlock();
}
```

Using `finally` ensures the lock is always released, even if an exception occurs within the critical section.

---

### **Spin Locks and Busy-Waiting**

A **spin lock** is a lock where a thread repeatedly checks if the lock is available, staying in a busy loop (or "spinning") until it can acquire the lock. This is useful when the lock is expected to be held for a short period, but it's inefficient on uniprocessor systems because the CPU is busy waiting.

Spin locks are discussed in the context of low-level locking mechanisms like **Peterson’s algorithm**, which is a classical mutual exclusion algorithm. However, spin locks are less practical on modern multiprocessors, where you would use **blocking locks** for better performance.

---

### **Java Memory Model (JMM)**

The Java Memory Model defines how threads interact through memory and how changes made by one thread are visible to others. It introduces concepts like:
- **Visibility**: Ensuring that changes made by one thread are visible to others.
- **Volatile Variables**: A variable declared as `volatile` ensures that changes made to it are visible to all threads immediately.

```java
private volatile boolean flag = true;
```

---

### **Time and Events in Distributed Systems**

In the context of distributed systems, events in different processes or threads can happen out of order due to communication delays. Leslie Lamport’s seminal paper "Time, Clocks, and the Ordering of Events in a Distributed System" introduces:
- **Logical Clocks**: These help to establish an ordering of events without relying on physical clocks, critical in ensuring consistency in distributed systems.

---

### **Key Terms**

- **Atomicity**: Operations are atomic if they are completed as a single unit without any other thread interfering.
- **Deadlock**: A situation where two or more threads are blocked forever, waiting for each other.
- **Liveness**: Refers to the system's ability to continue executing operations without getting stuck in some inactive state (like deadlock or starvation).

---

## Lecture 02 : Shared Memory I


## Lecture 03 : Shared Memory II


## Lecture 04 : Testing & Verification


## Lecture 05 : Lock-Free Data Structures


## Lecture 06 : Linearizability

