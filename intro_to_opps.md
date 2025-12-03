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

