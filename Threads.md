#  Threads

---

## 1 What is a Process?

A **process** is:

> A program in execution.

Examples inside MS Word:

* Displaying text
* Spell checking
* Auto-saving

---

## 2 What is a Thread?

A **thread** is:

> A lightweight process or a single unit of execution.

Threads allow a program to do **multiple tasks simultaneously**.

---

## 3 Why Threads?

Earlier programs ran **sequentially**:

```
Task 1 → Task 2 → Task 3
```

Threads allow:

```
Task 1  
Task 2  
Task 3  
(All running concurrently)
```

---

## 4 Concurrency vs Parallelism

### **Concurrency**

Multiple tasks appear to run at the same time
(but the CPU switches very fast).

### **Parallelism**

Multiple CPU cores executing tasks at the same time.

---

## 5 CPU Analogy — Lanes & Cars

* **Thread = Car**
* **Core = Lane**

Within a lane → cars are processed in sequence
Across lanes → cars run in parallel

Rules:

* One core executes only **one thread** at a time
* Multiple threads may exist, but CPU switches between them

---

# 5.6 Creating Threads — Hello World Example

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Hello from thread");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start();  // starts a new thread
    }
}
```

---

# 5.7 Print 1 to 100 Using Threads

```java
class PrintNumbers extends Thread {
    public void run() {
        for (int i = 1; i <= 100; i++) {
            System.out.println(i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        new PrintNumbers().start();
    }
}
```

# Program vs Process vs Thread

---

## 1. Program vs Process

### **Program**

* A passive set of instructions.
* Example: `MS Word.exe` stored on disk.

### **Process**

* A **program in execution**.
* When you open MS Word → a process is created.
* Each process runs in its own memory space.

---

## 2. Thread

A **thread** is the smallest unit of execution inside a process.

* A process can have **multiple threads** running tasks in parallel.
* Example (MS Word):

  * One thread for displaying text
  * One thread for spell-check
  * One thread for auto-save

### Analogy

* **Process = Car**
* **Thread = Tasks inside car (like driver, AC, music)**

---

## 3. CPU, Core & Threads

* A **core** can execute **one thread at a time**.
* The **operating system** decides *which thread runs when*.
* But each thread can be assigned **multiple tasks**.

### Example

```
[Core]
   ↓ executes 1 thread at a time
[Thread] → multiple tasks handled one by one
```

### Example CPU

A **2.4 GHz** CPU performs ~**2.4 billion operations per second**.

---

# 4. Concurrency vs Parallelism

---

## **Sequential System**

```
Task1 → Task2 → Task3
```

---

## **Concurrency**

> Multiple tasks in progress **at different stages** at the same time.
> They *appear* to run together but may not make progress together.

Example:

* Pawan is watching a lecture
* Pawan scrolls Instagram
* Pawan is also eating

He switches focus → **context switching**.

---

## **Parallelism**

> Multiple tasks making progress **at the same time**, literally in parallel.

Requires:

* Multi-core CPU

---

## Relationship

| Condition                                      | Meaning        |
| ---------------------------------------------- | -------------- |
| If a system is **parallel**, is it concurrent? | **Yes**        |
| If a system is **concurrent**, is it parallel? | **Not always** |

---

# 5. Context Switching

When CPU switches from one thread to another:

* Takes time
* Causes delay
* Threads resume from where they stopped

Example:

```
Pawan watches lecture → OS switches → scroll Instagram → OS switches → eats
```

---

# 6. Memory, Processes & Threads

Every process has:

* Its own memory
* A **PCB (Process Control Block)**

### PCB stores:

* Register values
* Program counter (line number to execute next)
* State of the process
* Memory details

### CPU Execution

The OS brings necessary instructions & data from RAM into **cache** for faster execution.

---

# 7. Creating Your Own Thread in Java

## Important Rule

**Think in terms of tasks**, not threads.

### Step 1 — Identify tasks

For each task that must run in parallel → create a class.

### Step 2 — Class should implement `Runnable`

This marks the class as a task for threads.

```java
class HelloWorldPrinter implements Runnable {
    @Override
    public void run() {
        System.out.println("Hello World");
    }
}
```

### Step 3 — Run the thread

```java
public class Main {
    public static void main(String[] args) {

        Runnable task = new HelloWorldPrinter();
        Thread t1 = new Thread(task);

        t1.start();  // starts a new thread
    }
}
```

---

# 8. Example Output Behavior

Depending on CPU scheduling:

```
main thread prints 1
Hello World (from t1)
main thread prints 2
Hello World 2 (if multiple threads created)
```

Threads interleave randomly:

```
HW
1
HW2
2
HW
```

This randomness is expected in multithreading.

---
Here is your content **cleaned, structured, and converted into a clear `.md` format**, while keeping the original meaning intact.

---

# Agenda

1. Print numbers **1 to 100**, each from a **separate thread**
2. **Executor** and **ThreadPool**
3. Print numbers **1 to 100** using thread pool
4. **Callables**
5. How threads return data to parent thread
6. **Multithreaded Merge Sort**
7. **Futures**

---

# Print Numbers 1 to 100 — Each from a Separate Thread

### **Problem**

Print each number (1–100) from its own thread.

---

## Step 1: Identify the tasks

We need **100 tasks**, each task prints a number.

---

## Step 2: Create a `NumberPrinter` class

```java
class NumberPrinter implements Runnable {
    int numberToPrint;

    public NumberPrinter(int numberToPrint) {
        this.numberToPrint = numberToPrint;
    }

    @Override
    public void run() {
        System.out.println(numberToPrint);
    }
}
```

---

## Step 3: Main program to start 100 threads

```java
public class Client {
    public static void main(String[] args) {

        for (int i = 1; i <= 100; i++) {
            NumberPrinter np = new NumberPrinter(i);
            Thread t = new Thread(np);
            t.start();
        }
    }
}
```

---

# Problem with This Approach

### ❌ Creating 100 threads is expensive

### ❌ CPU will waste time in **context switching**

### ❌ On enterprise servers, thousands of requests may come per second

### ❌ Creating a thread per request = huge overhead

---

# Why Thread Pools?

### Analogy

Production lines in a factory:

* EARLIER:
  Task → Create new worker → Worker does task
* NOW (efficient):
  Task → Give to existing workers (pool of workers)

---

# Thread Pool Benefits

* Limits number of threads
* Reduces CPU overhead
* Reduces context switching
* Improves system stability

---

# Thread Pool Example (ExecutorService)

```java
ExecutorService service = Executors.newFixedThreadPool(10);

for (int i = 1; i <= 100; i++) {
    NumberPrinter task = new NumberPrinter(i);
    service.submit(task);
}

service.shutdown();
```

Now only **10 threads** will handle all **100 tasks**, not 100 threads.

---

# How Many Threads in a Pool?

### Formula:

```
threads = number_of_cores * 2
```

Because of **hyperthreading**, each core can run **2 threads**.

### CPU Example

| CPU Cores | Max Threads |
| --------- | ----------- |
| 2 cores   | 4 threads   |
| 4 cores   | 8 threads   |
| 8 cores   | 16 threads  |

---

# When Are We Underutilizing the CPU?

A thread runs a task until finished.
If the task involves:

* File I/O
* Network calls
* DB calls
* Disk read/write

➡️ **CPU becomes idle**
➡️ Waiting for external operations
➡️ Other threads cannot run on that thread

That’s why thread pools help schedule tasks efficiently.

---

# Types of Tasks

There are two categories:

| Type              | Example               | CPU Utilization |
| ----------------- | --------------------- | --------------- |
| **CPU-Intensive** | calculations, sorting | High            |
| **I/O-Intensive** | network, file, DB     | Low             |

Thread pool size differs based on task type.

---

# Callables, Futures & Synchronization

## Agenda

1. **Callables** – How threads return data
2. **Synchronization Problem** – Adder & Subtractor

---

# 1. Callables

## Why do we need Callables?

* A `Runnable` **cannot return data** back to the main thread.
* A `Callable<T>` **returns data** of type `T` after finishing its execution.

---

# What is a Callable?

### Runnable vs Callable

| Runnable              | Callable             |
| --------------------- | -------------------- |
| Returns nothing       | Returns a value      |
| No `throws Exception` | Can throw exceptions |
| `run()` method        | `call()` method      |

Callable represents a **task** that returns a result.

---

## Steps to create a Callable

### **1. Identify the task**

Example: Merge two sorted arrays

```java
class MergeSorter { ... }
```

### **2. Identify the return type**

Example: A sorted list

```java
Callable<List<Integer>>
```

### **3. Implement `Callable<T>`**

```java
class MergeSorter implements Callable<List<Integer>> {
    @Override
    public List<Integer> call() {
        // merge-sort logic
        return mergedList;
    }
}
```

---

# Submitting Callables to ExecutorService

```java
ExecutorService service = Executors.newFixedThreadPool(4);
Future<List<Integer>> future = service.submit(new MergeSorter());
```

### What does `submit()` return?

* ❌ NOT actual data
* ✔️ It returns a **Future<T>**
* Future is a placeholder for data that will arrive later.

---

# Future

Future represents:

* A value that will be available **later**
* Returned immediately when you call `submit()`
* Real result can be retrieved using `future.get()`

Example:

```java
Future<String> f1;
Future<List<Integer>> f2;
```

### Working

```
Main thread ---- submits task ----> Thread Pool  
    ↓                                ↓
 receives Future<T>             executes task  
                                    ↓  
                                produces result  
```

---

# Example Thread Pool Behaviour

Thread pool (4 threads):

```
pool threads → [7, 3, 11, 6, 4, 2, 5, 8, 19]
tasks executing in parallel
results returned to main thread through Futures
```

Main thread then collects results:

```
Future 1 → result  
Future 2 → result  
...
```

---

# 2. Synchronization Problem

### Problem

We have a shared variable:

```
count = 0
```

Two tasks run **in parallel**:

1. Add numbers 1 to 100
2. Subtract numbers 1 to 100

Expected final value:

```
0
```

---

# Race Condition Example

### Both threads:

* Read the same value
* Modify it
* Write back
  Causes **inconsistent results**.

Illustration:

| Step    | Adder Thread | Subtractor Thread |
| ------- | ------------ | ----------------- |
| Read    | count = 1    | count = 1         |
| Add/Sub | count = 2    | count = 0         |
| Write   | stores 2     | stores 0          |

Final count becomes incorrect.

---

# Why Does This Happen?

Because both threads are doing:

```
read → modify → write
```

simultaneously on the same memory location.

This is called a **synchronization problem**.

---

# Visualization

```
Shared variable: count = 0

Adder thread ----+
                  | both accessing count
Subtractor -------+
```

Without synchronization:

* Operations overlap
* Updates are lost
* Final output becomes unpredictable

---

# Summary

###  Callables

* Allow methods to return data from threads
* Use `call()`
* Submitted to thread pools
* Return `Future<T>`

###  Futures

* Represent results that will come later
* Retrieve using `get()`

###  Synchronization Problem

* Occurs when multiple threads access shared data
* Leads to inconsistent results
* Must use synchronization (locks/atomic variables) to fix

---

# Synchronization

## Agenda

1. When does the synchronization problem occur?
2. What is an ideal solution?
3. Solutions

   * Mutex Locks
   * `synchronized` keyword (Java-specific)
   * Semaphores

---

# 1. When Does a Synchronization Problem Occur?

A synchronization problem occurs when:

> **More than one thread works on the same shared data at the same time**,
> causing **wrong / inconsistent results**.

###  Critical Section (CS)

A **critical section** is a part of the code where shared data is accessed or modified.

If **two or more threads** enter the CS at the same time → **synchronization problem**.

### Example

Two threads run in parallel:

```java
class Adder { ... }
class Subtractor { ... }
```

Both modify:

```
count = count + 1
count = count - 1
```

At the same time → inconsistent output.

---

# 2. Race Condition

A **race condition** occurs when:

> Multiple threads try to enter the **critical section simultaneously**.

This leads to unpredictable and incorrect results.

### Example Sequence (Adder & Subtractor)

| Operation | Adder Thread | Subtractor Thread |
| --------- | ------------ | ----------------- |
| Read      | count = 1    | count = 1         |
| Modify    | count + 1    | count - 1         |
| Write     | 2            | 0                 |

Depending on interleaving, **final count** becomes wrong.

---

# 3. Preemption (Very Important)

Preemption = CPU **interrupts** a running thread and switches to another thread.

### Why dangerous?

Even if a thread is inside the critical section, it can be paused:

```
Thread executes → (CS) → CPU switches → another thread enters CS → corruption
```

On a **single-core CPU**, preemption amplifies the problem.

---

# Example of Preemption

```
Thread A → print Hi
Thread A → read count
    (CPU switches)
Thread B → print Hi
Thread B → read count
...
```

Both threads read the same count → wrong updates.

---

# Ideal Solution Requirements

A good synchronization mechanism must satisfy:

###  1. Mutual Exclusion

Only **one thread** should access the critical section at a time.

###  2. Progress

The system should always keep making progress.
Threads must not be blocked forever.

###  3. Bounded Waiting

No thread should wait indefinitely to enter the CS.

###  4. No Busy Waiting

A thread should **not repeatedly check in a loop** ("busy waiting") if it can enter the CS.

---

# Busy Waiting (Bad Example)

```
while(lock == taken) {
    // keep checking
}
```

This wastes CPU time.

---

# Ideal Solution

A mechanism that:

* Blocks threads when CS is locked
* Wakes them up automatically
* Avoids busy waiting

---

# 4. Mutex Lock (Mutual Exclusion Lock)

A **mutex lock** is an object that ensures only **one thread** can enter the critical section.

### Behaviour

* Thread must **acquire lock** before entering CS
* Thread must **unlock** after exiting
* Only one thread can hold the lock at a time
* Other threads **wait** automatically
* No busy waiting
* Ensures mutual exclusion

---

## Example Concept Diagram

```
Shared Data (count)

     +-------- lock --------+
     |                      |
Adder Thread        Subtractor Thread
     |                      |
 acquire lock        acquire lock (waits)
     |                      |
 read-modify-write   read-modify-write
     |                      |
 unlock             acquires lock next
```

---

# 5. Properties of a Lock

1. **Mutual Exclusion:**
   Only one thread can enter the CS.

2. **Automatic Waiting:**
   Other threads wait until lock is released.

3. **Notification:**
   When lock is released, waiting threads are notified.

4. **No Busy Waiting:**
   Threads sleep instead of continuously checking.

---

# 6. Code Structure (Generalized)

### Case 1 – No Lock (Wrong)

```java
for(...) {
    // CS
    count++;
}
```

### Case 2 – With Lock (Correct)

```java
lock.lock();
try {
    count++;
} finally {
    lock.unlock();
}
```

---

# Summary

| Concept              | Meaning                                                      |
| -------------------- | ------------------------------------------------------------ |
| **Critical Section** | Shared data access region                                    |
| **Race Condition**   | Multiple threads modify same data simultaneously             |
| **Preemption**       | CPU switches threads mid-operation                           |
| **Mutex Lock**       | Ensures only one thread enters CS                            |
| **Ideal Solution**   | Mutual exclusion, progress, bounded waiting, no busy waiting |

---








