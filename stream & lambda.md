# Java 8 — Streams, Lambdas & Functional Interfaces

# Agenda

1. Functional Interfaces
2. Lambda Expressions
3. Streams
4. Stream Operations (Intermediate + Terminal)
5. Parallel Streams
6. Why Streams? Why Lambdas?
7. Best Examples

---

# 1. Functional Interfaces

A **functional interface** is an interface with **exactly one abstract method**.

Examples:

* `Runnable` → `run()`
* `Callable<T>` → `call()`
* `Comparable<T>` → `compareTo()`
* `Comparator<T>` → `compare(x, y)`
* `Predicate<T>` → `test(T)`
* `Supplier<T>` → `get()`
* `Consumer<T>` → `accept(T)`
* `Function<T, R>` → `apply(T)`

### Explanation

> A functional interface is like a **socket with one pin** —
> you can plug in ONE behavior.

---

##  How to create your own

```java
@FunctionalInterface
interface Calculator {
   int operate(int a, int b);
}
```

---

# 2. Lambda Expressions

### Lambda = Short way to write implementation of a functional interface.

### General Syntax

```
(parameters) -> { body }
```

Examples:

```java
Runnable r = () -> System.out.println("Running in thread");
```

```java
Comparator<Integer> comp = (a, b) -> a - b;
```

---

## Explanation

> A lambda is like sending someone a **ready-made instruction card** instead of creating a whole new class.

You used to write:

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Hello");
    }
}
```

Now simply:

```java
Runnable r = () -> System.out.println("Hello");
```

---

##  Why Lambdas?

* Less boilerplate
* More readable
* Faster coding
* Great for parallel execution
* Basis for Streams API

---

# 3. Streams

### A Stream is **a pipeline of operations performed on a collection/data-source**.

Examples of data sources:

* List
* Set
* Arrays
* Files
* Database rows
* Kafka messages (conceptually)

---

## Explanation

> A Stream is like a **water pipe**:
>
> * Water = data
> * Filters = intermediate operations
> * Tap = terminal operation
>
> Once water passes the tap → stream is **closed**.

---

# 4. Why Streams?

### ✔ Clean and readable

Old way:

```java
for(Product p : products) {
    if(p.price > 1000) {
        System.out.println(p.name);
    }
}
```

Stream way:

```java
products.stream()
        .filter(p -> p.price > 1000)
        .map(p -> p.name)
        .forEach(System.out::println);
```

### Lazy Evaluation

Stream runs only when a **terminal operation** is called.

### Parallel Execution

Just add:

```java
products.parallelStream()
```

to use all CPU cores.

---

# 5. Terminology

| Type                       | Meaning                             |
| -------------------------- | ----------------------------------- |
| **Intermediate Operation** | Returns a stream → chainable        |
| **Terminal Operation**     | Ends the stream → produces a result |

---

# 6. Intermediate Operations

### ✔ filter()

```java
list.stream().filter(n -> n % 2 == 0)
```

### ✔ map()

```java
list.stream().map(x -> x * x)
```

### ✔ sorted()

```java
list.stream().sorted()
```

### ✔ distinct()

```java
list.stream().distinct()
```

### ✔ limit(), skip()

```java
list.stream().skip(2).limit(5)
```

---

# 7. Terminal Operations

### ✔ forEach()

```java
stream.forEach(System.out::println);
```

### ✔ collect()

```java
List<String> names = stream.collect(Collectors.toList());
```

### ✔ reduce()

```java
int sum = list.stream().reduce(0, (a, b) -> a + b);
```

### ✔ count()

```java
list.stream().count();
```

### ✔ findFirst(), findAny()

```java
productStream.findFirst();
```

---

# 8. Streams Are One-time Use

Once consumed via terminal operation:

```java
Stream s = list.stream();
s.forEach(...);
s.forEach(...); // ❌ IllegalStateException
```

Streams **cannot** be reused.

---

# 9. Stream Example on Database-like Data

```java
List<Product> products = productDataSource.getAllProducts();

products.stream()
        .filter(p -> p.price > 1000)
        .map(p -> p.name)
        .forEach(System.out::println);
```

---

# 10. Good Real-World Examples

### **A. Read all lines from a file**

```java
Files.lines(Path.of("data.txt"))
     .filter(line -> line.contains("error"))
     .forEach(System.out::println);
```

---

### **B. Find top 3 highest-paid employees**

```java
employees.stream()
         .sorted((a,b) -> b.salary - a.salary)
         .limit(3)
         .forEach(e -> System.out.println(e.name));
```

---

### **C. Convert list of objects → map**

```java
Map<Integer, String> map =
    employees.stream()
             .collect(Collectors.toMap(e -> e.id, e -> e.name));
```

---

# 11. Parallel Streams (Important)

Use all CPU cores:

```java
list.parallelStream()
    .map(x -> heavyOperation(x))
    .forEach(System.out::println);
```

### Explanation

> Think of parallel streams like having **multiple chefs** working on different parts of your order simultaneously.

### When to use?

* Heavy CPU operations
* Large datasets
* Non-blocking tasks

---

# 12. Functional Interface + Lambda + Stream Together

```java
@FunctionalInterface
interface EligibilityCheck {
    boolean isEligible(Product p);
}

EligibilityCheck expensive = p -> p.price > 2000;

products.stream()
        .filter(expensive::isEligible)
        .map(p -> p.name)
        .forEach(System.out::println);
```

---

# 13. Summary Table

| Topic                    |      Meaning                                |
| ------------------------ | ------------------------------------------- |
| **Functional Interface** | A socket with one pin (one method)          |
| **Lambda**               | A small, quick instruction card             |
| **Stream**               | A pipeline: filters → transformers → output |
| **Intermediate Ops**     | Filters in pipeline                         |
| **Terminal Ops**         | The tap where output comes                  |
| **Parallel Streams**     | Multiple workers processing data            |

---


