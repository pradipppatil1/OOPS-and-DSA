# Synchronization, Synchronized Keyword & Semaphores

## Agenda

1. **synchronized keyword**
2. **Synchronized methods**
3. **Semaphores** (Producer–Consumer Problem)
4. **Atomic Data Types**

---

# 1. `synchronized` Keyword (Java)

> Imagine a bathroom with **one key**.
> Only one person can use it at a time.
> Others must **wait outside** until the key is free.

This is how `synchronized` works — it ensures that only **one thread** enters a critical section at a time.

---

## Example: Without Synchronization (Problem)

Two threads modify shared data:

```java
int count = 0;

Thread 1 → count = count + 1  
Thread 2 → count = count + 1
```

Both read the same initial value → **wrong result** (race condition).

---

## With `synchronized`

```java
synchronized(count) {
   // critical section
}
```

Think of this as:

```
lock(count) → do work → unlock(count)
```

Only **one thread** can enter this block at a time.

---

# 2. Synchronized Methods

If a method of a class is marked `synchronized`, then:

> Only one thread can run **any synchronized method** of that object at a time.

This means methods share the **same lock of that object**.

---

## Example

```java
class Counter {
    private int value;

    synchronized void increment() {
        value++;
    }

    synchronized void decrement() {
        value--;
    }

    int getValue() {
        return value;
    }
}
```

### Observations

```java
Counter c1 = new Counter();
Counter c2 = new Counter();
```

| Condition                           | Will they run in parallel? | Why?                                |
| ----------------------------------- | -------------------------- | ----------------------------------- |
| `c1.increment()` & `c1.decrement()` | ❌ No                       | Same object → same lock             |
| `c1.increment()` & `c1.getValue()`  | ✔ Yes                      | `getValue()` is NOT synchronized    |
| `c1.increment()` & `c2.increment()` | ✔ Yes                      | Different objects → different locks |

---

# Explanation

> Think of each object as a **house** with its **own bathroom key**.
> Only one person can use that house’s bathroom,
> but another house has its own bathroom — so two houses can work independently.

---

# 3. Producer–Consumer Problem (Using Semaphores)

### Story

Imagine a shirt store:

* It has **limited shelves** (max 6 shirts).
* **Producers** place shirts on shelves.
* **Consumers** take shirts from shelves.

Rules:

1. A consumer can enter only if **a shirt exists**.
2. A producer can enter only if **shelf space exists**.

Locks alone are not enough → because we need a way to keep **count** of:

* available shirts
* empty spaces

This is where **semaphores** come in.

---

# 4. Semaphores

> A semaphore is like a **counter-based lock**.

### Difference Between Lock & Semaphore

| Lock                      | Semaphore                  |
| ------------------------- | -------------------------- |
| Only 1 thread allowed     | Can allow **N** threads    |
| Think: **1 bathroom key** | Think: **N parking slots** |
| No counting               | Counts available resources |

---

## Basic Semaphore

```java
Semaphore s = new Semaphore(2);
```

This means:

* At most **2 threads** can access the resource at once.
* Thread must call `.acquire()` before entering.
* Must call `.release()` after leaving.

---

# Producer–Consumer Using Semaphores

### We need 3 things:

1. **Semaphore emptySlots**

   * How many shelves are empty
   * Initial value = max shelves (e.g., 6)

2. **Semaphore filledSlots**

   * How many shirts are present
   * Initial value = 0

3. **Mutex lock**

   * To protect the actual list while adding/removing

---

## Example Code (Simple)

```java
Semaphore empty = new Semaphore(6); // empty shelves
Semaphore full = new Semaphore(0);  // no shirts initially
Semaphore lock = new Semaphore(1);  // mutual exclusion

List<String> store = new ArrayList<>();
```

### Producer

```java
empty.acquire();   // Wait for empty space
lock.acquire();    // Enter critical section

store.add("shirt");

lock.release();    // Exit CS
full.release();    // A shirt is available
```

### Consumer

```java
full.acquire();    // Wait for a shirt
lock.acquire();    // Enter critical section

store.remove(0);

lock.release();    // Exit CS
empty.release();   // A shelf is empty
```

---

# Producer–Consumer Visual

### Shelves:

```
[ ][ ][ ][ ][ ][ ]   max = 6
```

### Producers (P) add shirts

### Consumers (C) take shirts

Semaphores:

```
emptySlots = 6
filledSlots = 0
```

Flow:

1. Producer adds → emptySlots = 5, filledSlots = 1
2. Consumer takes → emptySlots = 6, filledSlots = 0

System stays stable.

---

# 5. Semaphore Examples 

### Example 1: Parking Lot (Semaphore 4)

```
Semaphore parking = new Semaphore(4);
```

There are **4 parking spots**.
Only **4 cars** can park.
5th car must **wait**.

---

### Example 2: Gym Machines

```
Semaphore machines = new Semaphore(10);
```

10 treadmills → 10 people can run.
11th person waits until someone finishes.

---

### Example 3: Restaurant Tables

```
Semaphore tables = new Semaphore(20);
```

20 tables → 20 groups can sit.

---

# 6. Why Not Use Only Locks?

Locks only prevent **simultaneous access**,
but they **do not count resources**.

Producer–Consumer needs resource counting.

Hence:

* Locks = **mutual exclusion**
* Semaphores = **mutual exclusion + counting**

---

# Summary

| Concept                  | ELI5 Explanation                                |
| ------------------------ | ----------------------------------------------- |
| **synchronized**         | One person using bathroom → one thread in CS    |
| **synchronized methods** | House-level bathroom key (object-level lock)    |
| **Semaphore**            | Parking lot with N slots                        |
| **Producer–Consumer**    | Shirts in store → producers add, consumers take |
| **emptySlots**           | Seats available                                 |
| **filledSlots**          | Items available                                 |
| **Mutex**                | Only one person can access the shelf at a time  |

---

#  Semaphores, Atomic Types, ConcurrentHashMap & Deadlocks

---

## #️⃣ Agenda

1. Implementing Semaphore in Producer–Consumer
2. Atomic Data Types
3. Concurrent HashMap
4. Deadlock
5. Advanced Java Concurrency Concepts

---

#  1. Understanding Atomic Data Types

### ❓ Why do we need Atomic Types?

A normal integer update like:

```java
count = count + 1;
```

is **NOT atomic**.
It internally breaks into 3 steps:

1. Read count
2. Add 1
3. Store back

If multiple threads run this at the same time → **race condition**.

###  What is Atomic Operation?

> **An atomic operation is something the CPU performs in ONE single step.**

###  Example Problem (Non-Atomic Operation)

Two threads incrementing the same counter:

```java
int count = 0;

count++; // NOT atomic
```

You may expect final result = 2000 (for 2 threads running 1000 loops)
But you often get 1500, 1800, etc. → **lost updates**.

---

##  Atomic Data Types in Java

Java provides:

* `AtomicInteger`
* `AtomicBoolean`
* `AtomicLong`
* `AtomicReference`

These guarantee **thread-safe operations without needing locks**.

###  Example: Using AtomicInteger

```java
AtomicInteger count = new AtomicInteger(0);

count.incrementAndGet();  
```

Internally uses **CAS (Compare-And-Swap)** — lock-free, thread-safe.

###  Pros

* No need for `synchronized`
* Very fast due to lock-free algorithm

###  Cons

* Slightly more overhead than primitive operations
* Still need locks for grouped multi-variable operations

---

# 2. Concurrent Data Structures

When multiple threads access a normal `HashMap`, the internal structure can get corrupted.
To avoid that, Java proposed:

###  Problem With Normal HashMap + Locks

Java synchronised Map locks the **entire map**:

```java
Collections.synchronizedMap(map);
```

This causes **bad performance** because only **one thread** can access the map at a time.

---

## ConcurrentHashMap

> **Imagine a colony of lockers**.
>
> A normal HashMap has **one big locker** → only 1 person can access it.
>
> `ConcurrentHashMap` has **many small lockers (buckets)** → many people can access different lockers at the same time.

###  How ConcurrentHashMap improves performance?

* Locks **only a bucket**, not the whole map.
* Multiple threads can read/write simultaneously.

### Example:

```java
ConcurrentHashMap<Integer, String> map = new ConcurrentHashMap<>();
map.put(1, "A");
map.put(2, "B");
```

---

#  3. Deadlock

### ❓ What is a deadlock?

> Two threads are waiting for each other forever.

### Analogy

Two people trying to pass through a narrow door from opposite sides —
Both block each other → no one moves.

###  Deadlock Example

```java
synchronized(lock1) {
    synchronized(lock2) {
        // ...
    }
}

synchronized(lock2) {
    synchronized(lock1) {
        // ...
    }
}
```

Thread 1
→ Takes lock1 → waits for lock2
Thread 2
→ Takes lock2 → waits for lock1

Both **stuck forever**.

### Ways to Handle Deadlock

| Strategy      | Meaning                                            |
| ------------- | -------------------------------------------------- |
| **Avoidance** | Design so deadlock never happens                   |
| **Recovery**  | Detect & restart threads                           |
| **Ignorance** | JVM just hopes it never happens (default behavior) |

---

#  4. Semaphore (Producer–Consumer Problem)

Semaphore = **a counter + blocking mechanism**
Controls how many threads can access a resource.

### Example

> Think of a public toilet with 3 cabins.
> Only 3 people can enter at a time.
> Others must wait outside.

---

##  Semaphore Types

### 1. **Counting Semaphore**

* Allows `N` threads inside
* Example: Parking lot with 100 slots

### 2. **Binary Semaphore**

* Only 1 thread allowed
* Similar to a lock (mutex)

---

##  Producer–Consumer using Semaphore (Example)

### Requirements

* Producer must wait if buffer is full
* Consumer must wait if buffer is empty

### Semaphores Used

* `emptySpaces` → counts empty slots
* `fullSpaces` → counts filled slots
* `mutex` → ensures only one thread updates buffer at a time

### Java Example

```java
Semaphore empty = new Semaphore(5);  // buffer size 5
Semaphore full = new Semaphore(0);
Semaphore mutex = new Semaphore(1);

Queue<Integer> buffer = new LinkedList<>();
```

### Producer

```java
class Producer implements Runnable {
    public void run() {
        try {
            while (true) {
                empty.acquire();       // wait for empty space
                mutex.acquire();       // lock buffer

                buffer.add(1);
                System.out.println("Produced");

                mutex.release();
                full.release();        // signal item available
            }
        } catch (Exception e) {}
    }
}
```

### Consumer

```java
class Consumer implements Runnable {
    public void run() {
        try {
            while (true) {
                full.acquire();        // wait for available item
                mutex.acquire();       // lock buffer

                buffer.remove();
                System.out.println("Consumed");

                mutex.release();
                empty.release();       // signal empty space
            }
        } catch (Exception e) {}
    }
}
```

---

#  5. Advanced Concepts

### Generics

Used to create reusable & type-safe classes.

Example:

```java
class Pair<T, U> {
    T first;
    U second;
}
```

---

# Summary

| Concept               | Explanation                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------ |
| **Atomic Types**      | A safe calculator that never makes mistakes, even when many kids press buttons at the same time. |
| **ConcurrentHashMap** | Many lockers instead of one big locker → multiple people can store and retrieve items at once.   |
| **Semaphore**         | Toilet/parking counter controlling how many can enter.                                           |
| **Deadlock**          | Two people blocking each other in a doorway.                                                     |
| **Generics**          | A box that can hold anything but still keeps type safety.                                        |

---


