# The Four Pillars of OOP

## 1. Abstraction

**Short Explanation:**
Abstraction means showing only essential information to the user and hiding the complex implementation details. It focuses on **what** an object does, rather than **how** it does it.

**Example:**
When you drive a car, you use the accelerator pedal. You know what it does (makes the car go faster), but you don't need to know how the engine converts fuel and air into motion. The pedal is the abstraction of that complex process.

**Java Context:**
Achieved using **Abstract Classes** and **Interfaces**.

---

## 2. Encapsulation

**Short Explanation:**
Encapsulation is the practice of bundling data (variables) and the methods (functions) that operate on that data into a single unit (a Class). It also involves **restricting direct access** to the data (using private variables) and providing controlled access through **getter and setter** methods.

**Example:**
Think of a medical capsule. It contains the medicine (data) inside its shell (the class). You cannot directly access or manipulate the medicine; the capsule controls how you interact with it.

**Java Context:**
Achieved using **access modifiers** (private, public, protected) and getter/setter methods to protect data from unauthorized changes.

---

## 3. Inheritance

**Short Explanation:**
Inheritance allows a new class (child/subclass) to derive properties and behavior from an existing class (parent/superclass). This promotes **code reusability** and establishes an **"is-a" relationship**.

**Example:**
A `Dog` class can inherit from an `Animal` class. The Dog gets generic characteristics of Animal (like `eat()` and `sleep()`), and can also have its own (`bark()`).
A **Dog is-an Animal**.

**Java Context:**
Achieved using the **extends** keyword.
Java supports **single inheritance** for classes.

---

## 4. Polymorphism

**Short Explanation:**
Polymorphism means **many forms**. It allows a method or object to behave differently based on the context. The same method name can produce different results depending on the object calling it.

**Example:**
A method `draw()` behaves differently for different shapes:

* `Circle.draw()` → draws a circle
* `Square.draw()` → draws a square

Same method name, different behavior.

**Java Context:**
Two types:

### **Compile-time (Static) Polymorphism**

* Achieved through **Method Overloading** (same method name, different parameters)

### **Run-time (Dynamic) Polymorphism**

* Achieved through **Method Overriding** (subclass provides specific implementation of a parent method)

---
# Java Access Modifiers

## Access Modifier Comparison Table

| Access Modifier             | Accessible Within Same Class | Accessible Within Same Package | Accessible in Subclass (Different Package) | Accessible From Anywhere |
| --------------------------- | ---------------------------- | ------------------------------ | ------------------------------------------ | ------------------------ |
| **public**                  | ✔️ Yes                       | ✔️ Yes                         | ✔️ Yes                                     | ✔️ Yes                   |
| **protected**               | ✔️ Yes                       | ✔️ Yes                         | ✔️ Yes (through inheritance)               | ❌ No                     |
| **default** *(no modifier)* | ✔️ Yes                       | ✔️ Yes                         | ❌ No                                       | ❌ No                     |
| **private**                 | ✔️ Yes                       | ❌ No                           | ❌ No                                       | ❌ No                     |

---

## Quick Notes

* **public** → Accessible everywhere.
* **protected** → Accessible in same package + subclasses in other packages.
* **default** → Accessible only within the same package.
* **private** → Accessible only within the same class.
---

#  Java OOP

## 1. Copy Constructor

### ✔️ Definition

A constructor that creates a new object by copying fields from an existing object.

### ✔️ Syntax

```java
public Student(Student old) {
    this.name = old.name;
    this.age = old.age;
}
```

### ✔️ Why is it needed?

* To create independent objects with same values
* To avoid unwanted shared references
* Useful when cloning or making backups of objects

### ✔️ Key Point

Java does **not** provide an automatic copy constructor — you must write it manually.

---

## 2. `this` Keyword

* Refers to **current object**
* Used to resolve naming conflicts
* Used to call other constructors (`this()`)
* Helps avoid ambiguity between local and instance variables

---

## 3. Java Data Types

### **Primitive Types**

Stored as raw values in stack memory.
Examples: `int`, `double`, `float`, `char`, `boolean`

### **Reference Types / Objects**

Stored in heap memory, variables hold **addresses**.
Examples: `String`, `Array`, `Custom objects`

---

## 4. Shallow Copy vs Deep Copy

### **Shallow Copy**

* New object points to the **same referenced fields**
* Changing one affects the other
* Example:

```java
Student st2 = st1;  // Not a real copy
```

### **Deep Copy**

* Complete duplication
* New object + new copies of all nested objects
* Requires manually copying every field

### **Important**

Java does **not** provide built-in deep copy for custom classes.

---

## 5. Strings & Memory

* Strings are **immutable**
* Changing a string creates a **new memory object**
* Example:

```java
st2.name = "Hello"; // New memory created
```

---

## 6. Pass by Value vs Pass by Reference

### ✔️ Very Important

**Java is always *Pass By Value*.**

Meaning:

* Primitive → value copied
* Object → **reference value** copied (not the object)

### Example

```java
void update(Student s) {
    s.name = "Ankur";
}
```

Calling:

```java
update(st1);
```

Result:

* Works because both reference variables point to the **same object**

---

## 7. Destructors & Garbage Collection

### Java

❌ No destructors
✔️ Automatic **Garbage Collector** frees unused objects
✔️ Runs automatically — cannot predict timing

### Other Languages

C++ uses destructors since memory must be freed manually.

---

## 8. Inheritance

### ✔️ Definition

A mechanism where one class inherits fields and methods of another.

### ✔️ Keywords

`extends` → class inheritance

### ✔️ Example

```
Animal  
 └── Mammal  
      └── Dog  
```

### ✔️ Benefits

* Code reuse
* Clean hierarchy
* Easy maintainability

---

## 9. Polymorphism

### **Compile-time Polymorphism**

* Method Overloading
* Resolved by compiler

### **Runtime Polymorphism**

* Method Overriding
* Resolved by JVM at runtime

### Example

```java
Animal a = new Dog();
a.speak();  // Dog's method executed
```

---

## 10. Abstraction (Quick Recap)

### ✔️ Definition

Hiding internal details and showing only essential behavior.

### ✔️ Achieved Using

* Abstract classes
* Interfaces

---

# 🔥 Final Quick Summary (Fast Revision)

| Topic            | Key Point                            |
| ---------------- | ------------------------------------ |
| Copy Constructor | Creates new object from existing one |
| Shallow Copy     | Shares data (dangerous)              |
| Deep Copy        | Independent objects                  |
| Pass By Value    | Java always pass-by-value            |
| Destructors      | Not in Java (GC handles cleanup)     |
| Inheritance      | “Is-a” relationship                  |
| Polymorphism     | One interface, many behaviors        |
| Abstraction      | Show essential, hide complexity      |
| Strings          | Immutable, new memory on change      |

---

# Agenda

* Inheritance
* Polymorphism
* Interfaces

---

# Inheritance

### Key Concepts

* A constructor of the **parent class** knows best how to initialize its own attributes.
* When creating a child class object, constructors are called **top–down** from parent → child.

### **Object Creation Flow**

Steps when creating an object:

```java
D d = new D();
```

Order of constructor calls:

1. `A` (top-most parent)
2. `B`
3. `C`
4. `D` (child class)

**Calling order (constructor execution):**
`A → B → C → D`

---

# Polymorphism

### Meaning

"Poly" = many
"Morph" = forms
**Polymorphism = something that has many forms**

Example:

```java
Animal a = new Dog();
```

* You can store a **child object** in a variable of the **parent data type**.
* The client interacts only with **generic properties**, not specific ones.

---

## Rules of Polymorphism

### Compiler Checks:

Compiler checks the **data type of the reference variable**, not the object.

Example:

```java
A a = new C();
```

* Compiler allows access only to methods/variables of **A**, even though object is `C`.

---

# Why Polymorphism?

* Increases **reusability**
* Makes code more **generic**
* Examples:

```java
List<Integer> list1 = new ArrayList<>();
List<Integer> list2 = new LinkedList<>();
```

Both behave as `List` even though actual objects are different.

---

# Types of Polymorphism

## 1. Compile-Time Polymorphism

###  Method Overloading

### Rules:

Methods are **overloaded** when:

* Same method name
* Different method signatures (parameters)

Examples:

```java
void hello() { }
void hello(String name) { }
```

Another example:

```java
void printHello() { }
void printHello(String s) { }
void printHello(Integer i) { }
void printHello(String name, int age) { }
```

### Notes:

* Return type does NOT matter for overloading
* Overloading is resolved at **compile time**

---

## 2. Runtime Polymorphism

### ✔️ Method Overriding

### Rules:

Method is **overridden** when:

* Same method name
* Same return type
* Same parameters
* Parent → Child class relationship exists

Example:

```java
class A {
    void doSomething(String a) {
        System.out.println("Hello");
    }
}

class B extends A {
    void doSomething(String c) {
        System.out.println("Bye");
    }
}
```

Usage:

```java
A a = new A();
a.doSomething("x");   // Hello

A b = new B();
b.doSomething("x");   // Bye
```

### Key Points:

* Compiler checks **reference type**
* JVM at runtime checks **actual object**
* Therefore: Method overriding = **runtime polymorphism**

---

# Example: Multiple Subclasses

```
A  
├── B  
├── C  

A.doSomething() → Hello  
B.doSomething() → Bye  
C.doSomething() → Hi  
```

Calling:

```java
A obj = new C();
obj.doSomething();
```

Output → `Hi`

Because JVM uses **object type**, not reference type.

---

# Interfaces

* Provide highest abstraction
* Classes implement interfaces
* Used for standardizing behavior across unrelated classes

---

# Final Key Takeaways

| Concept          | Compile-Time   | Runtime     |
| ---------------- | -------------- | ----------- |
| Overloading      | ✔️             | ✖️          |
| Overriding       | ✖️             | ✔️          |
| Decision made by | Compiler       | JVM         |
| Depends on       | Reference type | Object type |

---

# Agenda

1. **Interfaces**
2. **Abstract Classes**
3. **Static Keyword**

---

# 1. Interfaces

## What is a Class vs Interface?

### Class → Blueprint of **entities**

* Has **attributes (variables)**
* Has **behaviours (methods)**
* Methods contain **definitions (implementation)**
* Example: `Cat`, `Dog`

### Interface → Blueprint of **behaviours**

* Contains **only method declarations**
* No implementation inside interface methods
* No attributes (except `public static final` constants)
* Helps categorize classes based on behaviour
* Example: "Anyone who can walk, eat, run is an Animal"

---

## Example: Interface

```java
interface Animal {
    void eat();     // declaration
    void walk();    // declaration
    void run();     // declaration
}
```

---

## Implementing an Interface

### Cat Class

```java
class Cat implements Animal {

    public void eat() {
        System.out.println("Cat is eating");
    }

    public void walk() {
        System.out.println("Cat is walking");
    }

    public void run() {
        System.out.println("Cat is running");
    }

    public void meow() {
        System.out.println("Cat says Meow!");
    }
}
```

### Dog Class

```java
class Dog implements Animal {

    public void eat() {
        System.out.println("Dog is eating");
    }

    public void walk() {
        System.out.println("Dog is walking");
    }

    public void run() {
        System.out.println("Dog is running");
    }

    public void bark() {
        System.out.println("Dog says Bark!");
    }
}
```

### Rule

❗ **If a class implements an interface, it MUST implement all the methods declared in the interface.**

---

# 1.1 Interfaces in Real-World Example (Stacks)

```java
interface Stack {
    void push(int val);
    int pop();
    boolean isEmpty();
}
```

### ArrayStack Implementation

```java
class ArrayStack implements Stack {
    int top = -1;
    int[] arr = new int[10];

    public void push(int val) {
        arr[++top] = val;
    }

    public int pop() {
        return arr[top--];
    }

    public boolean isEmpty() {
        return top == -1;
    }
}
```

### LinkedListStack Implementation

```java
class LinkedListStack implements Stack {

    class Node {
        int data;
        Node next;
    }

    Node top;

    public void push(int val) {
        Node node = new Node();
        node.data = val;
        node.next = top;
        top = node;
    }

    public int pop() {
        int val = top.data;
        top = top.next;
        return val;
    }

    public boolean isEmpty() {
        return top == null;
    }
}
```

---

## 1.2 Interface Programming Principle (Very Important)

### **“Code to an Interface, not to an Implementation.”**

Why?

* Flexible
* Easy to extend
* Easy to replace implementation
* Low coupling

### Real Example: PhonePe & Bank APIs

PhonePe used YesBank APIs. When YesBank went down, PhonePe easily switched to ICICI Bank APIs **because their code used an Interface**, not a specific bank class.

---

### Interface Example

```java
interface BankAPI {
    double checkBalance(long accountNo);
    boolean transferMoney(String from, String to, double amount);
}
```

### YesBank Implementation

```java
class YesBankAPI implements BankAPI {
    public double checkBalance(long accountNo) { ... }
    public boolean transferMoney(String from, String to, double amount) { ... }
}
```

### ICICIBank Implementation

```java
class ICICIBankAPI implements BankAPI {
    public double checkBalance(long accountNo) { ... }
    public boolean transferMoney(String from, String to, double amount) { ... }
}
```

### PhonePe Using Interface

```java
class PhonePe {
    BankAPI api;   // Interface reference

    PhonePe(BankAPI api) {
        this.api = api;
    }

    void pay() {
        api.transferMoney("A", "B", 1000);
    }
}
```

---

# 2. Abstract Classes

## What is an Abstract Class?

* Contains **attributes + methods**
* Can have both:

  * **abstract methods (no body)**
  * **normal methods (with body)**
* Cannot be instantiated directly

---

## When to Use Abstract Classes?

Use abstract classes when:

* You want to provide **common attributes**
* You want child classes to decide **how to implement some behaviours**
* Example: All animals walk, but each animal walks differently

---

## Example

```java
abstract class Animal {
    String name;
    int age;

    abstract void walk();
    abstract int noOfLegs();

    void sleep() {
        System.out.println("Animal is sleeping");
    }
}
```

### Tiger Class

```java
class Tiger extends Animal {

    void walk() {
        System.out.println("Tiger is walking");
    }

    int noOfLegs() {
        return 4;
    }
}
```

### Cat Class

```java
class Cat extends Tiger {

    int noOfLegs() {
        return 4;
    }
}
```

### Usage

```java
Animal a = new Tiger();
a.walk();  // Tiger is walking
```

---

# 2. What is Multiple Inheritance?

Multiple Inheritance means:

> **A child class extending more than one parent class.**

Example (NOT allowed in Java):

```java
class D extends B, C { }  // ❌ Not allowed
```

## Why is it not allowed?

Because of the **Diamond Problem**.

### Diamond Problem Structure

```
     A
   /   \
  B     C
   \   /
     D
```

If both B and C have a method `doSomething()`, then:

```java
D.doSomething();
```

The compiler does **not know** whether to call B's version or C's version.

**Therefore, Java does NOT support multiple inheritance using classes.**

---

# 3. When do we use Abstract Classes?

Use an abstract class when:

* You don't want to create an object of the parent class.
* Child classes must implement some common behaviours.

Example:

```java
abstract class Animal {
    abstract void walk();
}

class Cat extends Animal {
    void walk() { System.out.println("Cat walks"); }
}

Animal a = new Cat();  // Allowed
Animal b = new Animal(); // ❌ Not allowed
```

---

# 4. Static Keyword

Static means:

> Something that belongs to the **class**, not to objects.

---

## 4.1 Static Variables

* Shared across all objects
* Loaded when the class is loaded
* One copy for the entire application

Example:

```java
class Student {
    int age = 25;
    static String universityName = "Scaler";
}
```

Usage:

```java
System.out.println(Student.universityName);
```

---

## 4.2 Static Methods

Rules:

* Can be called **without creating an object**
* Can **ONLY access static variables/methods**
* Cannot use `this` or `super`

Example:

```java
class MyClass {
    static void getAge() { ... }
}
```

Calling:

```java
MyClass.getAge();
```

---

## 4.3 Real Example

```java
class Roles {
    final static int kitchenWidth = 10;
    public static String TA = "Teaching Assistant";
    public static String Mentor = "House Mentor";
}
```

Usage:

```java
System.out.println(Roles.TA);
System.out.println(Roles.kitchenWidth);
```

---







