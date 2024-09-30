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
    - [**Thread-Safe Classes**](#thread-safe-classes)
      - [**Key Concepts in Thread Safety:**](#key-concepts-in-thread-safety)
      - [**Designing a Thread-Safe Class**:](#designing-a-thread-safe-class)
    - [**Safe Publication**](#safe-publication)
      - [**Safe Publication Methods**:](#safe-publication-methods)
      - [**Example: Safe Publication with `volatile` and `final`**:](#example-safe-publication-with-volatile-and-final)
    - [**Semaphores**](#semaphores)
      - [**How Semaphores Work:**](#how-semaphores-work)
      - [**Types of Semaphores**:](#types-of-semaphores)
      - [**Java Example: Using Semaphore**](#java-example-using-semaphore)
      - [**Semaphores and Producer-Consumer Problem**:](#semaphores-and-producer-consumer-problem)
    - [**Barriers**](#barriers)
      - [**How Barriers Work**:](#how-barriers-work)
      - [**Java Example: Using CyclicBarrier**](#java-example-using-cyclicbarrier)
      - [**Use Cases for Barriers**:](#use-cases-for-barriers)
    - [**Instance Confinement**](#instance-confinement)
      - [**How It Works**:](#how-it-works)
      - [**Example of Thread Confinement**:](#example-of-thread-confinement)
      - [**Advantages of Instance Confinement**:](#advantages-of-instance-confinement)
      - [**Instance Confinement in Real-World Scenarios**:](#instance-confinement-in-real-world-scenarios)
    - [**Producer-Consumer Problem**](#producer-consumer-problem)
      - [**Solution: Using Blocking Queues**](#solution-using-blocking-queues)
      - [**Why `BlockingQueue` is Ideal for Producer-Consumer**:](#why-blockingqueue-is-ideal-for-producer-consumer)
    - [**Summary of Key Concepts**](#summary-of-key-concepts)
  - [Lecture 04 : Testing \& Verification](#lecture-04--testing--verification)
    - [**Program Correctness in Concurrent Programming**](#program-correctness-in-concurrent-programming)
    - [**Concurrency Properties: Safety vs. Liveness**](#concurrency-properties-safety-vs-liveness)
    - [**Testing Concurrent Programs**](#testing-concurrent-programs)
      - [**Challenges in Testing Concurrent Programs**:](#challenges-in-testing-concurrent-programs)
      - [**Testing Strategies**:](#testing-strategies)
    - [**JUnit 5 for Testing Concurrent Programs**](#junit-5-for-testing-concurrent-programs)
    - [**Tools for Testing and Verification**](#tools-for-testing-and-verification)
    - [**Deadlocks and Testing for Them**](#deadlocks-and-testing-for-them)
    - [**Bounded Buffer**](#bounded-buffer)
      - [**Key Concepts of Bounded Buffer**:](#key-concepts-of-bounded-buffer)
    - [**Implementation of Bounded Buffer**](#implementation-of-bounded-buffer)
      - [**Semaphore-Based Bounded Buffer**](#semaphore-based-bounded-buffer)
      - [**Explanation**:](#explanation)
      - [**JUnit 5 Test for Bounded Buffer**:](#junit-5-test-for-bounded-buffer)
      - [**Explanation**:](#explanation-1)
    - [**Summary**](#summary)
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

### **Thread-Safe Classes**

A **thread-safe class** is a class where **concurrent execution** of methods and field accesses does not result in **data races**. This means that the class ensures correct behavior when accessed from multiple threads simultaneously.

#### **Key Concepts in Thread Safety:**

1. **Data Race**:
   - A data race occurs when two threads access the same shared memory location, at least one access is a write, and the accesses are not ordered by the **happens-before relationship**. If a data race occurs, the behavior of the program becomes non-deterministic.

2. **Class State**:
   - The class state refers to the fields defined in the class. For a class to be thread-safe, we need to ensure that no two threads can access or modify the shared state (class fields) concurrently without synchronization.

3. **Mutual Exclusion**:
   - Achieving thread safety requires enforcing mutual exclusion, meaning that only one thread can access the critical section at a time. In Java, mutual exclusion can be enforced using synchronization mechanisms like the `synchronized` keyword or explicit locks such as `ReentrantLock`.

4. **Escaping**:
   - A class is **safe from escaping** when it ensures that shared state does not leak out of the class, where it could be accessed without synchronization. This is typically done by making fields `private` and providing controlled access through methods that are properly synchronized.

#### **Designing a Thread-Safe Class**:
To analyze whether a class is thread-safe, ensure that:
- **No concurrent access** to shared state results in data races.
- **Shared fields** are either properly synchronized or are **immutable** (unchanging after creation).
- Use of **locks** or **synchronization mechanisms** ensures visibility of updates across threads.

---

### **Safe Publication**

**Publication** refers to making an object accessible to other threads. If an object is not published safely, another thread may see it in an incomplete or inconsistent state.

#### **Safe Publication Methods**:
1. **Using `volatile`**:
   - Declaring a field as `volatile` ensures visibility guarantees across threads. When one thread writes to a `volatile` variable, all subsequent reads by other threads will see that updated value.

2. **Using `final`**:
   - Declaring fields as `final` guarantees that they are properly constructed and visible to other threads after the object is fully initialized. Once a final field is assigned during object construction, it cannot be changed, making it a reliable way to publish shared data safely.

3. **Immutable Objects**:
   - An object is **immutable** if its state cannot be modified after creation. If an object is immutable, it is inherently thread-safe, as there is no risk of inconsistent updates or visibility issues.

4. **Thread Confinement**:
   - If an object is only used by one thread and never shared, it is **thread-confined** and does not require synchronization.

#### **Example: Safe Publication with `volatile` and `final`**:
```java
public class SafeInitialization {
    private volatile int x; // ensures visibility across threads
    private final Object o; // ensures safe publication of this object

    public SafeInitialization() {
        x = 42;
        o = new Object();
    }
}
```
In this example:
- `x` is volatile, meaning changes are immediately visible to all threads.
- `o` is final, ensuring that once it’s initialized, all threads will see it correctly.

---

---

### **Semaphores**

A **semaphore** is a synchronization primitive that can control access to a shared resource by multiple threads. It is a flexible mechanism that restricts the number of threads that can access a resource simultaneously.

#### **How Semaphores Work:**
- A semaphore maintains a **permit count**. 
  - A thread can acquire a permit (decrementing the count) if the count is greater than 0.
  - If no permits are available (count is 0), the thread blocks until another thread releases a permit (increments the count).
  
- **Semaphores** are often used to limit access to resources such as database connections or to control access to sections of code, such as limiting the number of active threads performing a task at once.

#### **Types of Semaphores**:
1. **Binary Semaphore**: This behaves like a lock (with values 0 and 1). A thread can acquire the semaphore (set it to 0) and release it (set it to 1), ensuring mutual exclusion.
2. **Counting Semaphore**: This allows up to a specified number of threads to acquire the semaphore simultaneously. For example, a semaphore initialized with `n` permits can allow up to `n` threads to access a critical section concurrently.

#### **Java Example: Using Semaphore**
```java
import java.util.concurrent.Semaphore;

class SemaphoreExample {
    private final Semaphore semaphore = new Semaphore(3); // allow 3 threads to access

    public void performTask() {
        try {
            semaphore.acquire(); // acquire a permit
            System.out.println(Thread.currentThread().getName() + " acquired the semaphore.");
            // Critical section (shared resource access)
            Thread.sleep(2000); // Simulating some work
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            System.out.println(Thread.currentThread().getName() + " releasing the semaphore.");
            semaphore.release(); // release the permit
        }
    }
}

public class Main {
    public static void main(String[] args) {
        SemaphoreExample example = new SemaphoreExample();

        for (int i = 0; i < 5; i++) {
            new Thread(example::performTask).start();
        }
    }
}
```
In this example:
- Only 3 threads can access the critical section at a time, as the semaphore allows 3 permits.
- The remaining threads will block until a permit becomes available.

#### **Semaphores and Producer-Consumer Problem**:
Semaphores can be used to solve the **Producer-Consumer problem**, where one semaphore tracks empty slots in a buffer, and another tracks filled slots.

---

### **Barriers**

A **barrier** is a synchronization construct that allows multiple threads to wait for each other at a specific point before proceeding. It ensures that all threads reach a certain point before any of them continue execution. Barriers are useful in scenarios like parallel computing, where tasks need to wait for each other at various stages.

#### **How Barriers Work**:
- **CyclicBarrier**: In Java, the `CyclicBarrier` is used as a barrier. It allows a fixed number of threads to wait for each other to reach the barrier, and once all threads reach the barrier, the barrier is "broken" and all threads proceed.
- The term "cyclic" means that the barrier can be reused after it has been broken.

#### **Java Example: Using CyclicBarrier**
```java
import java.util.concurrent.CyclicBarrier;

class BarrierExample implements Runnable {
    private final CyclicBarrier barrier;

    public BarrierExample(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            System.out.println(Thread.currentThread().getName() + " is waiting at the barrier.");
            barrier.await(); // waiting at the barrier
            System.out.println(Thread.currentThread().getName() + " has crossed the barrier.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("All parties have reached the barrier, let's proceed!"));

        Thread t1 = new Thread(new BarrierExample(barrier));
        Thread t2 = new Thread(new BarrierExample(barrier));
        Thread t3 = new Thread(new BarrierExample(barrier));

        t1.start();
        t2.start();
        t3.start();
    }
}
```
In this example:
- Three threads wait at the barrier (`barrier.await()`).
- Once all threads reach the barrier, they are allowed to proceed, and the barrier can be reused.

#### **Use Cases for Barriers**:
- Barriers are typically used in parallel algorithms where different threads must synchronize at certain computation phases before moving forward.
- They are commonly used in scientific computing and parallel simulations.

---

### **Instance Confinement**

**Instance confinement** is a concurrency technique that involves ensuring that data is only accessed by a single thread. By confining an object or resource to a single thread, you avoid the need for synchronization, since no other thread will access the shared state.

#### **How It Works**:
- The confined object is never shared between threads. It’s only accessed within the context of the thread that owns it.
- Since no other threads can access the object, there is no need for explicit locking or synchronization.
  
#### **Example of Thread Confinement**:
Suppose you have an object that holds temporary data for processing. Instead of making this object shared between multiple threads, you can create a new instance of the object for each thread.

```java
public class ThreadLocalExample {
    private static final ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 0);

    public static void main(String[] args) {
        Runnable task = () -> {
            int value = threadLocal.get();
            value += 1;
            threadLocal.set(value);
            System.out.println(Thread.currentThread().getName() + ": " + threadLocal.get());
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        Thread t3 = new Thread(task);

        t1.start();
        t2.start();
        t3.start();
    }
}
```
In this example:
- The `ThreadLocal` class confines the data to each thread. Each thread has its own version of the `threadLocal` variable, so they do not interfere with one another.

#### **Advantages of Instance Confinement**:
- Simplifies reasoning about thread safety by eliminating shared state.
- Reduces the need for synchronization mechanisms (locks, semaphores, etc.), leading to potentially better performance.

#### **Instance Confinement in Real-World Scenarios**:
- **Database connections**: When multiple threads access a database, each thread may have its own connection, confining the connection instance to the thread.
- **Thread-local storage**: Similar to the above example, using thread-local storage ensures that each thread has its own instance of a variable, avoiding synchronization overhead.

---

### **Producer-Consumer Problem**

The **Producer-Consumer Problem** is a classical example of a **synchronization problem**. It involves two types of threads:
- **Producer**: A thread that generates items and puts them into a shared resource (like a queue).
- **Consumer**: A thread that consumes (removes) items from the shared resource.

The goal is to ensure that:
- The producer doesn't try to add an item if the queue is full.
- The consumer doesn't try to consume from an empty queue.

#### **Solution: Using Blocking Queues**

Java provides the `BlockingQueue` interface, which simplifies the implementation of the producer-consumer problem. A `BlockingQueue` supports operations that wait if the queue is full (for producers) or empty (for consumers).

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

class Producer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        try {
            while (true) {
                int item = produce();
                queue.put(item); // Blocks if the queue is full
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }

    private int produce() {
        // Producing logic here
        return 1;
    }
}

class Consumer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        try {
            while (true) {
                int item = queue.take(); // Blocks if the queue is empty
                consume(item);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }

    private void consume(int item) {
        // Consuming logic here
    }
}

public class ProducerConsumerExample {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new LinkedBlockingQueue<>(10); // Capacity 10
        Thread producerThread = new Thread(new Producer(queue));
        Thread consumerThread = new Thread(new Consumer(queue));

        producerThread.start();
        consumerThread.start();
    }
}
```

In this example:
- **`BlockingQueue.put()`** will block if the queue is full, preventing the producer from overfilling the buffer.
- **`BlockingQueue.take()`** will block if the queue is empty, preventing the consumer from trying to consume when there are no items.

#### **Why `BlockingQueue` is Ideal for Producer-Consumer**:
- It handles the synchronization logic internally, preventing race conditions without requiring explicit locking mechanisms.
- It avoids **busy-waiting** (where threads continually check if they can proceed), improving efficiency.

---

### **Summary of Key Concepts**

- **Thread-Safe Class**: A class where no concurrent access results in data races. Ensuring thread safety involves synchronizing access to shared fields, using locks, or ensuring immutability.
- **Safe Publication**: Objects must be safely published to other threads. Using `volatile`, `final`, and immutability are effective techniques.
- **Semaphores**: Used to control access to a shared resource by multiple threads. Useful for limiting the number of concurrent threads accessing a resource.  
- **Barriers**: A synchronization mechanism that forces threads to wait until all have reached a certain point. Useful in parallel algorithms where tasks need to synchronize at specific phases.
- **Instance Confinement**: A technique that confines the use of objects or resources to a single thread, reducing the need for synchronization and simplifying thread-safety concerns.
- **Producer-Consumer Problem**: Involves managing synchronization between producers and consumers using a shared resource. `BlockingQueue` provides a simple and efficient way to solve this problem in Java.

---

## Lecture 04 : Testing & Verification

Here are thorough notes based on **Lecture 4** on **Testing & Verification**, drawing from the lecture slides and the reading material from *Java Concurrency in Practice* Chapter 12, and additional resources like **JUnit 5 documentation**.

---

### **Program Correctness in Concurrent Programming**

A **concurrent program** is considered **correct** if it satisfies its specification. This can be broken down into two types of properties:
- **Safety**: Ensures that "something bad never happens."
  - Example: Two traffic lights at an intersection are never green at the same time.
  - In code, this could translate to ensuring shared resources like a counter or collection never reach an invalid state, e.g., a collection size being less than 0.

- **Liveness**: Guarantees that "something good eventually happens."
  - Example: A traffic light will eventually turn red.
  - In code, this might mean that operations like adding items to a collection are eventually successful, even in high contention environments.

Testing concurrent programs focuses on identifying **undesired interleavings**, which are referred to as **counterexamples** (examples of thread execution orders that violate correctness). Unlike sequential programs, concurrent program bugs are often **non-deterministic**, meaning they may not manifest in every run.

---

### **Concurrency Properties: Safety vs. Liveness**

1. **Safety Properties**:
   - The counterexample is a **finite interleaving** where the safety property is violated.
   - For example: If we have a property stating that a collection size must never be less than 0, a counterexample would be an interleaving where two threads simultaneously modify the collection in a way that makes the size negative.

2. **Liveness Properties**:
   - A counterexample here is an **infinite interleaving** where the property never holds.
   - For example: If a program guarantees that a thread will eventually acquire a lock, a counterexample would be an infinite execution where this thread is perpetually blocked (perhaps due to deadlock or starvation).

### **Testing Concurrent Programs**

#### **Challenges in Testing Concurrent Programs**:
- **Nondeterminism**: The order in which threads execute is unpredictable, and this unpredictability makes concurrent programs hard to test.
- **Interleaving Variability**: Since the actual thread interleavings that lead to errors are rare, triggering them reliably through testing is a significant challenge.
- **Race Conditions**: A common issue in concurrent programs where two or more threads access shared data concurrently without proper synchronization, leading to unpredictable results.

#### **Testing Strategies**:
1. **Functional Correctness Tests**:
   - These are standard tests that aim to verify the **correct behavior** of a concurrent program (similar to tests in sequential programming but adapted for concurrency).
   - Example: Testing that a **counter** class returns the correct value after multiple threads increment it concurrently.
   
2. **Stress Testing and High Contention**:
   - Stress testing is designed to force as many interleavings as possible by running many threads under high contention, increasing the likelihood of triggering race conditions or deadlocks.

3. **Repeated Execution**:
   - Due to the non-deterministic nature of concurrent programs, repeating tests multiple times (using JUnit’s `@RepeatedTest`) can help uncover concurrency bugs.

---

### **JUnit 5 for Testing Concurrent Programs**

JUnit 5 provides a simple framework for writing and organizing tests. Here's how you can use it for testing concurrent programs:

1. **Sequential Tests**:
   - These are standard unit tests that run sequentially and ensure basic functionality. In the context of concurrent programs, they can be useful for verifying that basic operations work in the absence of multithreading.

   Example:
   ```java
   @Test
   public void testingCounterSequential() {
       Counter counter = new CounterDR();
       int expectedValue = 0;
       for (int i = 0; i < 10_000; i++) {
           counter.inc();
           expectedValue++;
       }
       assertEquals(expectedValue, counter.get());
   }
   ```

2. **Concurrent Tests**:
   - JUnit 5 can be extended to support tests that simulate concurrent environments. You can create **multiple threads** that operate on a shared resource and verify its final state.

   Example:
   ```java
   @RepeatedTest(10)
   public void testingCounterParallel() throws InterruptedException {
       int N = 1000;
       int nrThreads = 10;
       Counter counter = new CounterDR();

       // Create and start multiple threads
       Thread[] threads = new Thread[nrThreads];
       for (int i = 0; i < nrThreads; i++) {
           threads[i] = new Thread(() -> {
               for (int j = 0; j < N; j++) {
                   counter.inc();
               }
           });
           threads[i].start();
       }

       // Wait for all threads to finish
       for (int i = 0; i < nrThreads; i++) {
           threads[i].join();
       }

       // Verify the final count
       assertEquals(N * nrThreads, counter.get());
   }
   ```
   In this test:
   - We create 10 threads, each incrementing the counter 1000 times.
   - We use `assertEquals` to verify that the final value of the counter is `N * nrThreads`.

3. **Testing with Blocking Queues**:
   JUnit is also used for testing common concurrency patterns like **producer-consumer** using blocking queues. For example, verifying that producers can insert into the queue and consumers can retrieve items concurrently.

   Example:
   ```java
   @RepeatedTest(10)
   public void producerConsumerTest() throws InterruptedException {
       BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
       ExecutorService executor = Executors.newFixedThreadPool(2);

       // Producer
       executor.submit(() -> {
           for (int i = 0; i < 10; i++) {
               queue.put(i);
           }
       });

       // Consumer
       executor.submit(() -> {
           for (int i = 0; i < 10; i++) {
               try {
                   queue.take();
               } catch (InterruptedException e) {
                   Thread.currentThread().interrupt();
               }
           }
       });

       executor.shutdown();
       executor.awaitTermination(1, TimeUnit.MINUTES);
       assertTrue(queue.isEmpty());
   }
   ```

   Here, the `BlockingQueue` is shared between producer and consumer threads, and the test verifies that the queue is empty after all items are processed.

---

### **Tools for Testing and Verification**

1. **Java PathFinder**:
   - This is a formal verification tool for Java programs that systematically explores all possible thread interleavings. It is used to check properties like deadlocks, race conditions, and general correctness in concurrent Java programs.

2. **FindBugs and Other Static Analysis Tools**:
   - Tools like **FindBugs** can be used to detect potential concurrency issues, such as missing synchronization and improper use of volatile variables.

---

### **Deadlocks and Testing for Them**

A **deadlock** occurs when two or more threads are blocked forever, waiting on each other to release locks. Deadlocks are challenging to detect since they may occur only under specific conditions (rare interleavings).

**Testing Deadlocks**:
- To catch deadlocks, stress tests can be written where many threads attempt to acquire locks in different orders, increasing the chances of detecting a deadlock scenario.

---

### **Bounded Buffer**

A **Bounded Buffer** is a classic concurrency problem where a fixed-size buffer is shared between a **producer** and a **consumer**. The producer inserts items into the buffer, and the consumer removes them. The challenge is to ensure that the buffer never overflows (producer tries to add to a full buffer) or underflows (consumer tries to remove from an empty buffer). This is typically solved using synchronization mechanisms like **semaphores** or **locks**.

#### **Key Concepts of Bounded Buffer**:
1. **Producer-Consumer Problem**:
   - The **producer** adds items to the buffer, and the **consumer** removes them.
   - The producer must block if the buffer is full.
   - The consumer must block if the buffer is empty.

2. **Bounded Buffer Properties**:
   - **Capacity**: The buffer has a fixed maximum size, known as its capacity.
   - **Blocking**: When the buffer is full, producers are blocked from inserting. Similarly, when the buffer is empty, consumers are blocked from removing items.

---

### **Implementation of Bounded Buffer**

In **Java**, a bounded buffer can be implemented using various concurrency utilities provided by the `java.util.concurrent` package. One common approach is using a **semaphore-based solution** or the **BlockingQueue** class, which provides built-in support for blocking on full or empty conditions.

Here is an example from *Java Concurrency in Practice* (Chapter 12):

#### **Semaphore-Based Bounded Buffer**
In this approach, we use semaphores to manage the availability of "slots" in the buffer:
- A **semaphore for empty slots** tracks how many spaces are left in the buffer.
- A **semaphore for filled slots** tracks how many items are available for consumption.

```java
// example from *Java Concurrency in Practice* (Chapter 12)
import java.util.concurrent.Semaphore;

public class BoundedBuffer<E> {
    private final Semaphore availableItems, availableSpaces;
    private final E[] items;
    private int putPosition = 0, takePosition = 0;

    public BoundedBuffer(int capacity) {
        availableItems = new Semaphore(0);
        availableSpaces = new Semaphore(capacity);
        items = (E[]) new Object[capacity];
    }

    public void put(E x) throws InterruptedException {
        availableSpaces.acquire(); // Wait if no space available
        doInsert(x);
        availableItems.release(); // Signal that an item is available
    }

    public E take() throws InterruptedException {
        availableItems.acquire(); // Wait if no items available
        E item = doExtract();
        availableSpaces.release(); // Signal that a space is available
        return item;
    }

    private synchronized void doInsert(E x) {
        items[putPosition] = x;
        putPosition = (putPosition + 1) % items.length;
    }

    private synchronized E doExtract() {
        E x = items[takePosition];
        takePosition = (takePosition + 1) % items.length;
        return x;
    }
}
```

#### **Explanation**:
1. **Semaphores**:
   - **availableSpaces** semaphore tracks the number of empty slots in the buffer.
   - **availableItems** semaphore tracks the number of filled slots (items ready for consumption).

2. **Blocking Behavior**:
   - **Producers** call `put(E x)`, which first checks if there is available space by acquiring the `availableSpaces` semaphore. If no space is available, the producer is blocked until a consumer removes an item.
   - **Consumers** call `take()`, which checks if there is an available item by acquiring the `availableItems` semaphore. If no item is available, the consumer is blocked until the producer adds a new item.

3. **Synchronized Methods**:
   - The actual insertion (`doInsert`) and extraction (`doExtract`) are **synchronized** to protect shared variables (`putPosition` and `takePosition`), ensuring thread safety.

#### **JUnit 5 Test for Bounded Buffer**:
Here’s how you might write a test to verify the behavior of the **Bounded Buffer**:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.concurrent.*;

public class BoundedBufferTest {
    @Test
    public void testPutAndTake() throws InterruptedException {
        BoundedBuffer<Integer> buffer = new BoundedBuffer<>(10);

        buffer.put(1);
        buffer.put(2);

        assertEquals(1, buffer.take());
        assertEquals(2, buffer.take());
    }

    @Test
    public void testBlockingOnFullBuffer() throws InterruptedException {
        BoundedBuffer<Integer> buffer = new BoundedBuffer<>(2);
        buffer.put(1);
        buffer.put(2);

        ExecutorService executor = Executors.newSingleThreadExecutor();
        executor.submit(() -> {
            try {
                buffer.put(3); // This will block because the buffer is full
                fail(); // We should never reach this if the buffer is blocking correctly
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        Thread.sleep(1000); // Wait for some time to ensure blocking
        assertEquals(1, buffer.take()); // This will unblock the put operation
        executor.shutdown();
    }
}
```

#### **Explanation**:
1. **Basic Test** (`testPutAndTake`):
   - This test verifies that the `put()` and `take()` operations work as expected: we put two integers in the buffer and take them out, ensuring that the correct values are returned.

2. **Blocking Test** (`testBlockingOnFullBuffer`):
   - This test verifies that the buffer blocks correctly when full. We attempt to `put()` a third item in a buffer with a capacity of 2, which should block the producer.
   - We then take an item from the buffer, which should unblock the producer and allow it to complete the `put()` operation.

---

### **Summary**

- Testing concurrent programs is a challenging task because of nondeterminism and the difficulty of reproducing bugs reliably.
- **Safety and liveness properties** form the foundation of reasoning about program correctness.
- JUnit 5 is an effective tool for writing both sequential and concurrent tests. By leveraging multiple threads and repeated execution, you can increase the chances of finding concurrency bugs.
- Formal tools like **Java PathFinder** can help in verifying correctness by exploring all possible interleavings of threads.
- - **Bounded Buffers** solve the classic producer-consumer problem by using synchronization primitives (like semaphores) to ensure that producers and consumers can safely share a fixed-size buffer.
- The **Semaphore-based implementation** blocks the producer when the buffer is full and blocks the consumer when the buffer is empty.

## Lecture 05 : Lock-Free Data Structures


## Lecture 06 : Linearizability

