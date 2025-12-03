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




