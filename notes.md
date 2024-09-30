# Notes : Practical Concurrent and Parallel Programming Autumn 2024 @ ITU

- [Notes : Practical Concurrent and Parallel Programming Autumn 2024 @ ITU](#notes--practical-concurrent-and-parallel-programming-autumn-2024--itu)
  - [Lecture 01 : Intro to Concurrency and Mutual Exclusion](#lecture-01--intro-to-concurrency-and-mutual-exclusion)
    - [**Introduction to Concurrency**](#introduction-to-concurrency)
      - [**Why Concurrency?**](#why-concurrency)
    - [**Java Threads**](#java-threads)
      - [**Basic Thread Creation in Java**](#basic-thread-creation-in-java)
    - [**Concurrency Challenges**](#concurrency-challenges)
    - [**Mutual Exclusion**](#mutual-exclusion)
      - [**Critical Sections**](#critical-sections)
    - [**Locks in Java**](#locks-in-java)
    - [**Spin Locks and Busy-Waiting**](#spin-locks-and-busy-waiting)
    - [**Java Memory Model (JMM)**](#java-memory-model-jmm)
    - [**Time and Events in Distributed Systems**](#time-and-events-in-distributed-systems)
    - [**Key Terms**](#key-terms)
  - [Lecture 02 : Shared Memory I](#lecture-02--shared-memory-i)
    - [**Shared Memory and Monitors**](#shared-memory-and-monitors)
    - [**Monitors**](#monitors)
      - [**Components of a Monitor:**](#components-of-a-monitor)
    - [**Java’s Synchronized Keyword**](#javas-synchronized-keyword)
      - [**Synchronized Methods**](#synchronized-methods)
      - [**Synchronized Blocks**](#synchronized-blocks)
    - [**Java Memory Model (JMM)**](#java-memory-model-jmm-1)
      - [**Visibility**](#visibility)
    - [**Volatile Variables**](#volatile-variables)
    - [**Reordering and Happens-Before Relationship**](#reordering-and-happens-before-relationship)
      - [**How Happens-Before Works:**](#how-happens-before-works)
      - [**Formally Reasoning with Happens-Before**](#formally-reasoning-with-happens-before)
    - [**Monitor Example: Readers-Writers Problem**](#monitor-example-readers-writers-problem)
      - [**Monitor Solution:**](#monitor-solution)
      - [**Fairness in the Readers-Writers Problem**](#fairness-in-the-readers-writers-problem)
      - [**Is the Monitor Example Fair?**](#is-the-monitor-example-fair)
      - [**How to Make the Solution Fair?**](#how-to-make-the-solution-fair)
    - [**Key Takeaways**](#key-takeaways)
  - [Lecture 03 : Shared Memory II](#lecture-03--shared-memory-ii)
  - [Lecture 04 : Testing \& Verification](#lecture-04--testing--verification)
  - [Lecture 05 : Lock-Free Data Structures](#lecture-05--lock-free-data-structures)
  - [Lecture 06 : Linearizability](#lecture-06--linearizability)


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

### **Shared Memory and Monitors**

**Shared memory** refers to memory that can be accessed by multiple threads in a concurrent program. When working with shared memory, synchronization is essential to ensure correct and predictable program behavior.

### **Monitors**
A **monitor** is a synchronization construct that allows threads to have both **mutual exclusion** and the ability to **wait** (block) until certain conditions are met. It's a combination of **shared state**, **methods**, and **synchronization mechanisms** in a structured way, allowing multiple threads to safely access shared resources.

#### **Components of a Monitor:**
1. **State (Shared Variables)**: 
   - These are variables (fields) shared between multiple threads. In concurrent programming, shared state needs to be protected to avoid inconsistent or incorrect results due to multiple threads modifying or reading the state simultaneously.
   - For example, in the **Readers-Writers problem**, the shared state would be:
     - A count of active readers (`int readers`).
     - A flag to indicate if a writer is active (`boolean writer`).

2. **Methods (Procedures)**:
   - These are the operations (or methods) that manipulate the shared state. In a monitor, these methods are synchronized to prevent multiple threads from executing critical sections at the same time. Each method in a monitor locks access to the shared state.
   - In Java, you can synchronize access using the `synchronized` keyword, ensuring that only one thread can execute the method or block at a time.

3. **Synchronization Mechanisms**:
   - Synchronization is used to enforce **mutual exclusion** so that only one thread can access a critical section of code at a time. This prevents race conditions.
   - In Java, this can be done using the `synchronized` keyword (which we’ll discuss in detail below) or using explicit locks such as `ReentrantLock`.

Monitors encapsulate the synchronization logic within objects, ensuring mutual exclusion at the method level. This concept was first introduced by **Tony Hoare** in 1974 and has been widely adopted in operating systems and programming languages.

**Components of a monitor:**
- **Lock (Mutual Exclusion):** Ensures that only one thread can execute a critical section at a time.
- **Condition Variables:** Allows threads to wait until a certain condition becomes true. It provides methods like `await()`, `signal()`, and `signalAll()` for controlling the execution flow of waiting threads.

---

### **Java’s Synchronized Keyword**

Java provides the `synchronized` keyword as a way to implement monitors for shared memory access. It ensures that only one thread can execute a synchronized block or method at any given time, thus providing mutual exclusion.

#### **Synchronized Methods**
A method can be marked as `synchronized`, meaning that only one thread can execute that method on a particular object at a time. Other threads trying to access the method will be blocked until the current thread exits the method.

Example:
```java
public synchronized void method() {
    // critical section
}
```

#### **Synchronized Blocks**
A more flexible way is using synchronized blocks, which allows you to specify the object whose lock is being used.

Example:
```java
public void method() {
    synchronized(this) {
        // critical section
    }
}
```

- **Synchronized keyword ensures:**
  - **Atomicity**: The code inside the synchronized block is executed as an atomic operation.
  - **Visibility**: Changes made by one thread inside a synchronized block are visible to others once the thread exits the block.
- **Synchronize on the object** whose fields (state) you are trying to protect.
- If you're trying to synchronize access across multiple objects or methods, it might make more sense to use **another shared object** or **a static lock object**.

---

### **Java Memory Model (JMM)**

The **Java Memory Model (JMM)** defines how threads interact through shared memory. It describes the rules by which memory operations (reads and writes) can be reordered and how values written by one thread become visible to other threads. Understanding JMM is key to reasoning about **visibility** and **reordering** in concurrent Java programs.

#### **Visibility**
Visibility ensures that changes made by one thread to shared variables are visible to other threads. Without synchronization, the updates made by one thread may not be visible to others due to:
- **Caching**: Variables might be cached locally by each thread, and the local cached value may not be written back to main memory immediately.
- **Reordering**: The compiler or processor might reorder instructions for performance reasons, which can lead to visibility issues.

---

### **Volatile Variables**

In Java, declaring a variable as **`volatile`** ensures visibility. When a variable is declared `volatile`, every thread that reads the variable will always see the latest value written by any other thread. This is crucial for variables that are accessed by multiple threads but not protected by locks.

Example:
```java
private volatile boolean flag = false;
```

- **Volatile guarantees visibility**: Writes to volatile variables are visible to other threads immediately after the write occurs. 
- **No reordering**: The compiler and processor are prohibited from reordering operations involving volatile variables.

---

### **Reordering and Happens-Before Relationship**

**Reordering** is the process where the compiler or processor changes the order of instructions to improve performance, as long as the final outcome remains the same. However, in concurrent programs, reordering can lead to inconsistent behavior.

Java’s **happens-before relationship** is a set of rules that dictate the ordering of reads and writes in a program. It guarantees that if one action happens-before another, the first is visible and ordered before the second. Key rules include:
- **Locks**: A call to `unlock()` happens-before any subsequent call to `lock()` on the same lock.
- **Volatile Variables**: Writes to a volatile variable happen-before reads of the same variable.
- **Thread Start and Join**: The start of a thread happens-before any actions in that thread, and actions in a thread happen-before another thread joins it.

#### **How Happens-Before Works:**
1. **Thread Start Rule**: A call to `Thread.start()` happens-before any action in the started thread.
2. **Thread Join Rule**: All actions in a thread happen-before another thread successfully returns from a `Thread.join()` on that thread.
3. **Synchronized Blocks/Methods**: An unlock on a monitor happens-before every subsequent lock on that same monitor. In simpler terms, when a thread exits a `synchronized` block, any updates made are visible to the next thread that enters a `synchronized` block on the same object.
4. **Volatile Variables**: A write to a `volatile` variable happens-before every subsequent read of that same variable by any thread.

#### **Formally Reasoning with Happens-Before**
To reason about the correctness of a concurrent program, you check whether the happens-before relationships cover all shared state interactions. You can ensure that:
- A write happens-before a read, meaning the updated value is visible to the reading thread.
- Synchronization constructs (like `synchronized` and `volatile`) are used to establish the necessary ordering between operations.

---

### **Monitor Example: Readers-Writers Problem**

The **Readers-Writers Problem** is a classic synchronization problem. Multiple threads (readers) can read from a shared resource simultaneously, but only one thread (a writer) can write to the resource at a time.

#### **Monitor Solution:**
1. Use a **lock** (`ReentrantLock`) to ensure mutual exclusion.
2. Define **conditions** to manage waiting and signaling between threads.
3. Use a **count** to track the number of active readers and a **boolean** to indicate if a writer is writing.

Here is a simplified version of how to implement this in Java:
```java
class ReadWriteMonitor {
    private int readers = 0;
    private boolean writer = false;
    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void readLock() throws InterruptedException {
        lock.lock();
        try {
            while (writer) {
                condition.await();
            }
            readers++;
        } finally {
            lock.unlock();
        }
    }

    public void readUnlock() {
        lock.lock();
        try {
            readers--;
            if (readers == 0) {
                condition.signalAll();
            }
        } finally {
            lock.unlock();
        }
    }

    public void writeLock() throws InterruptedException {
        lock.lock();
        try {
            while (readers > 0 || writer) {
                condition.await();
            }
            writer = true;
        } finally {
            lock.unlock();
        }
    }

    public void writeUnlock() {
        lock.lock();
        try {
            writer = false;
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }
}
```
In this example:
- Readers can read concurrently as long as no writer is writing.
- Writers must wait until there are no readers or writers accessing the shared resource.

#### **Fairness in the Readers-Writers Problem**

In the **Readers-Writers Problem**, **fairness** refers to ensuring that no thread (reader or writer) is **starved**. Starvation occurs when a thread waits indefinitely to access the resource because other threads are constantly being prioritized.

#### **Is the Monitor Example Fair?**
The current monitor-based solution for the Readers-Writers Problem, as described earlier, may **not be fair**. For instance:
- If writers are always given priority over readers, **readers** could be starved (if a writer keeps entering the critical section).
- Conversely, if readers are allowed to continuously read without giving a chance to writers, **writers** could be starved.

#### **How to Make the Solution Fair?**
To make it fair, we need to ensure that neither readers nor writers are indefinitely postponed. One possible approach is to use a **queue** to enforce **FIFO (first-in, first-out) order** for readers and writers, ensuring fairness.
An implementation of a fair readers-writer problem can be found in the lecture slides or in the group submission for Assignment 01 (not public).

Java’s `ReentrantLock` also supports **fair locks** via a constructor option:
```java
ReentrantLock lock = new ReentrantLock(true); // enables fairness
```
A fair lock ensures that threads acquire the lock in the order they requested it, avoiding starvation.

---

### **Key Takeaways**

1. **Monitors** encapsulate synchronization and provide mechanisms for blocking and signaling threads.
2. The **`synchronized` keyword** in Java ensures mutual exclusion and visibility, making it a fundamental tool for writing correct concurrent programs.
3. **Volatile variables** guarantee visibility across threads without locking, but they don’t guarantee atomicity.
4. Understanding **Java Memory Model** (JMM) is crucial for reasoning about visibility and reordering in concurrent programs.
5. The **happens-before relationship** provides a formal way to reason about the visibility and ordering of actions between threads.

--- 

## Lecture 03 : Shared Memory II


## Lecture 04 : Testing & Verification


## Lecture 05 : Lock-Free Data Structures


## Lecture 06 : Linearizability

