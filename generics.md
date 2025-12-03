# Java Generics & Collections

## Agenda

1. Generic Types
2. Raw Types
3. Generic Methods
4. Wildcards & Bounds
5. Generics + Inheritance
6. Collections Framework

---

# 1. Why Generics?

### What did we want?

1. Create a class that works for **any data type**
2. Still be **type-safe** (no wrong data inserted)

###  Generics introduced in **Java 5**

Before Java 5:

```java
List list = new ArrayList();
list.add("hello");
list.add(100); // also allowed 
```

No type safety → runtime errors.

After Java 5:

```java
List<String> list = new ArrayList<>();
list.add("hello");
list.add(100); // ❌ compile-time error
```

---

# What are Generics?

> **Think of a lunchbox.
> Writing "Snacks Only" on it stops kids from putting toys inside.**
>
> Similarly, `List<String>` tells Java:
> “Only store strings here.”

---

# 2. Generic Types (Creating Your Own)

### Template-like classes

Example: **Pair** class

```java
class Pair<F, S> {
    F first;
    S second;

    Pair(F first, S second) {
        this.first = first;
        this.second = second;
    }

    public F getFirst() { return first; }
    public S getSecond() { return second; }
}
```

### Usage

```java
Pair<Integer, Integer> p1 = new Pair<>(1, 2);
Pair<String, Float> p2 = new Pair<>("Height", 5.9f);
Pair<Student, String> p3 = new Pair<>(new Student(), "A+");
```

✔ Type-safe
✔ No casting
✔ Compiler guarantees correct types

---

# 3. Raw Types (Don’t Use!)

```java
List list = new ArrayList(); // raw type
list.add("PPP");
list.add(100); // allowed 
```

Raw type = **no generics** → behaves like storing everything as `Object`.

**Why still allowed?**
➡ Backward compatibility with pre-Java-5 code.

---

# 4. Generic Methods

Used mostly in **utility classes**.

```java
class MathUtil {
    public static <T> T printValue(T value) {
        System.out.println(value);
        return value;
    }
}
```

Usage:

```java
MathUtil.<String>printValue("Hi");
MathUtil.printValue(100); // type inference
```

---

# 5. Generics & Inheritance (Very Important)

This is where most developers get confused.

### ❌ Wrong

```java
List<Animal> animals = new ArrayList<Dog>(); // not allowed
```

Even though `Dog extends Animal`,
`List<Dog>` does **not** extend `List<Animal>`.

### ✔ Understanding

* `Dog extends Animal`
* But **List<Dog> DOES NOT extend List<Animal>**

Why?
➡ Because otherwise you could do:

```java
List<Animal> animals = new ArrayList<Dog>();
animals.add(new Cat()); // Cat into List<Dog> ❌
```

Hence Java forbids it.

---

# 6. Wildcards & Bounds

## 6.1 Upper Bound — `<? extends Type>`

```java
List<? extends Animal> animals
```

Meaning:

> Accepts List<Animal>, List<Dog>, List<Cat>, etc.

### But:

❌ You **cannot add** elements (except null).
✔ You **can read** elements as `Animal`.

### Example

> “I can read animals from this zoo,
> but I can’t add animals because I don’t know which species box this is.”

---

## 6.2 Lower Bound — `<? super Type>`

```java
List<? super Dog> dogs
```

Meaning:

> Accepts List<Dog>, List<Animal>, List<Object>

### You CAN add `Dog` objects safely.

### Example

> “This is a box that accepts dogs or anything above dog in family tree.”

---

## Wildcard Diagrams

```
        Animal
       /      \
   Dog        Cat
```

### Upper Bound `? extends Animal`

Allowed:

* List<Animal>
* List<Dog>
* List<Cat>

### Lower Bound `? super Dog`

Allowed:

* List<Dog>
* List<Animal>
* List<Object>

---

# 7. Examples From Notes

### ❌ Wrong

```java
doSomething(List<Animal> animals);

List<Dog> dogs = ...
doSomething(dogs); // ❌ mismatch
```

### ✔ Correct using wildcard

```java
void printAnimalNames(List<? extends Animal> animals) {
    for(Animal a : animals)
        System.out.println(a.getName());
}

List<Dog> dogs = ...
printAnimalNames(dogs); // ✔ works
```

---

# 8. addADog Example (Lower Bound)

```java
void addADog(List<? super Dog> list) {
    list.add(new Dog());
}
```

Works with:

* List<Dog>
* List<Animal>
* List<Object>

---

# 9. Collections API (Quick Overview)

### Common Interfaces

| Interface | Description                 |
| --------- | --------------------------- |
| **List**  | Ordered, duplicates allowed |
| **Set**   | Unique elements             |
| **Map**   | Key–value pairs             |
| **Queue** | FIFO / LIFO variants        |

### Implementations

| Interface | Implementations           |
| --------- | ------------------------- |
| List      | ArrayList, LinkedList     |
| Set       | HashSet, TreeSet          |
| Map       | HashMap, TreeMap          |
| Queue     | ArrayDeque, PriorityQueue |

---

# Summary

| Concept                |  Explanation                                       |
| ---------------------- | ------------------------------------------------------ |
| **Generics**           | Labels on boxes → ensures only correct items go inside |
| **Wildcard ? extends** | “I can read animals from this zoo, but can’t add”      |
| **Wildcard ? super**   | “I can add dogs to this box safely”                    |
| **Raw Type**           | A box without a label → anyone can put anything        |
| **Generic Method**     | A magical function that works for any type             |

---


