# Java Collections Framework 

## Agenda

1. What is a Collection?
2. Why do we use Collections?
3. Common Interfaces & Implementations
4. List, Set, Map
5. Vector
6. PriorityQueue
7. LinkedHashMap / LinkedHashSet
8. TreeMap / TreeSet
9. Comparable vs Comparator
10. Custom Sorting Examples

---

# 1. What is a Collection?

### Definition

The Collections Framework in Java provides **ready-made data structures** so we don’t need to build them ourselves.

### Explanation

> Imagine you have different types of storage boxes:
>
> * A **list box** (ordered items)
> * A **set box** (no duplicates)
> * A **map box** (key → value)
>
> Java gives you all these boxes already designed and tested so you don’t have to build them.

---

#  2. Why Use Collections?

### Problems with Arrays

* Fixed size
* No built-in search, sorting, reversing
* Hard to insert/delete in between
* Not memory efficient

### Collections solve these by giving:

✔ Dynamic size
✔ Powerful built-in methods
✔ Cached data structures
✔ Better performance-enhanced implementations

---

# 3. Main Interfaces in Java Collections

| Interface | What it Represents                    | Examples                        |
| --------- | ------------------------------------- | ------------------------------- |
| **List**  | Ordered collection, allows duplicates | ArrayList, LinkedList, Vector   |
| **Set**   | Unique elements, no duplicates        | HashSet, LinkedHashSet, TreeSet |
| **Map**   | key → value pairs                     | HashMap, TreeMap, LinkedHashMap |
| **Queue** | FIFO / Priority ordering              | PriorityQueue, ArrayDeque       |

---

# 4. List Implementations

## A. ArrayList

* Fast for **search**
* Slow for **inserting in the middle**
* Dynamic array

```java
List<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);
```

---

## B. LinkedList

* Fast for adding/removing at **beginning / middle**
* Slower for searching

```java
List<String> ll = new LinkedList<>();
ll.add("A");
ll.add("B");
```

---

## C. Vector (Thread-safe)

### Tread-safe:

> “Vector is thread-safe, but slow.”

### Why slow?

Because every method is `synchronized`.

```java
Vector<Integer> v = new Vector<>();
v.add(10);
v.add(15);
```

---

# 5. PriorityQueue (Very Important)

### Definition

A queue that stores items in **priority-based order**, not insertion order.

### Explanation

> Think of a **hospital emergency room**.
> The most critical patient is treated first, even if they came later.

---

### Example

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.add(10);
pq.add(5);
pq.add(15);

while(!pq.isEmpty()) {
    System.out.println(pq.poll());
}
```

Output:

```
5
10
15
```

Stores elements in **sorted order** by default (min-heap).

---

#  6. Set Implementations

## A. HashSet

* No order
* Super fast (constant time operations)
* No duplicates

```java
Set<String> set = new HashSet<>();
set.add("A");
set.add("B");
set.add("A"); // ignored
```

---

## B. LinkedHashSet

* Maintains **insertion order**

```java
Set<Integer> lhs = new LinkedHashSet<>();
lhs.add(10);
lhs.add(20);
lhs.add(30);
```

---

## C. TreeSet

* Maintains **sorted order**
* Uses **Red-Black Tree**

```java
Set<Integer> ts = new TreeSet<>();
ts.add(5);
ts.add(1);
ts.add(9);
```

Output:

```
1, 5, 9
```

---

#  7. Map Implementations

## A. HashMap

* Keys are stored in hashed structure
* Order NOT guaranteed

```java
Map<Integer, String> map = new HashMap<>();
map.put(1,"A");
map.put(2,"B");
```

---

## B. LinkedHashMap

* Maintains **insertion order**

```java
Map<Integer, String> map = new LinkedHashMap<>();
```

---

## C. TreeMap

* Keys stored in **sorted order**
* Uses Red-Black tree

```java
Map<String, Integer> tm = new TreeMap<>();
```

---

# 8. Comparable vs Comparator (Very Important)

## ❗ Why do we need sorting rules?

Example cities:

```
Delhi, Mumbai, Pune
```

What if you want to sort by:

* alphabetical?
* length?
* reverse order?

Java needs rules → **Comparable / Comparator**

---

#  9. Comparable

### Used when the **class itself defines its natural ordering**.

Example:
Integers → ascending
Strings → alphabetical

### Syntax

```java
class Student implements Comparable<Student> {
    int id;
    String name;

    public int compareTo(Student other) {
        return this.id - other.id; // sort by id
    }
}
```

---

### Explanation

> Comparable = “I know how to sort myself.”
> Comparator = “Someone else will tell me how to sort.”

---

# 10. Comparator (External Sorting Logic)

Use when:

* You want **multiple ways** to sort.
* You **cannot modify** the class.

### Example: Sort by name alphabetically

```java
class SortByName implements Comparator<Student> {
    public int compare(Student a, Student b) {
        return a.name.compareTo(b.name);
    }
}
```

Usage:

```java
Collections.sort(students, new SortByName());
```

---

# 11. Multiple Sorting Examples

### Sort by age

```java
class SortByAge implements Comparator<Student> {
    public int compare(Student a, Student b) {
        return a.age - b.age;
    }
}
```

### Sort by name length

```java
class SortByNameLength implements Comparator<Student> {
    public int compare(Student a, Student b) {
        return a.name.length() - b.name.length();
    }
}
```

---

#  12. Natural Ordering

* Integer → ascending
* String → alphabetical
* Boolean → false < true

---

# 13. Summary of Collections

| Concept           |  Explanation                                 |
| ----------------- | ------------------------------------------------ |
| **List**          | Like a playlist – ordered, allows duplicates     |
| **Set**           | Like roll numbers – no duplicates                |
| **Map**           | Like a dictionary – word → meaning               |
| **PriorityQueue** | Hospital emergency room – highest priority first |
| **Comparable**    | Object says “I know how to compare myself”       |
| **Comparator**    | External judge deciding sorting rules            |

---

