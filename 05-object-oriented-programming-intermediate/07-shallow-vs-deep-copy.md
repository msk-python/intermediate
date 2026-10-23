---
id: python-intermediate-shallow-vs-deep-copy
slug: shallow-vs-deep-copy
course: python-for-intermediate
chapter: "5: Object-Oriented Programming (Intermediate)"
topic: "5.7 Object Mutability: Shallow Copy vs Deep Copy"
title: "Shallow Copy vs Deep Copy in Python: The copy Module"
description: "Master Python object copying: assignment vs shallow copy vs deep copy, id() identity, nested mutable objects, copy.deepcopy(), and interactive quiz."
difficulty: Intermediate
readingTime: 9
order: 27
keywords:
  - python shallow copy
  - python deep copy
  - copy module
  - python object references
  - mutable objects python
lastUpdated: 2026-10-23
author: MSK Institute
version: 1.0.0
---

# 🐍 Object Mutability: Shallow Copy vs Deep Copy

In Python, variables do not store actual values—they store **references (memory addresses) to objects**. Failing to understand the difference between **assignment**, **shallow copying**, and **deep copying** is one of the most common causes of subtle data corruption bugs in intermediate Python programs.

---

## 🧩 1. Assignment (`=`) vs Copying

When you assign a variable using `=`, you **do not copy the object**. You merely create a new name pointing to the exact same object in memory:

```python
original = [10, 20, 30]
alias = original

alias.append(99)

print("Original:", original) # [10, 20, 30, 99]
print("Alias:   ", alias)    # [10, 20, 30, 99]
print(original is alias)     # True (identical memory address!)
```

---

## 📄 2. Shallow Copy (`copy.copy` or `.copy()`)

A **Shallow Copy** creates a new outer collection, but inserts references to the **same child objects** found in the original:

```python
import copy

original = [[1, 2, 3], [4, 5, 6]]
shallow = copy.copy(original)

# Modifying the outer container does NOT affect original:
shallow.append([7, 8, 9])
print(len(original)) # 2 (unaffected outer container)

# BUT modifying an inner nested list DOES affect both!
shallow[0].append(999)
print(original[0])   # [1, 2, 3, 999]  <-- ACCIDENTAL MUTATION!
```

Because the nested child lists were only referenced, both `original[0]` and `shallow[0]` point to the identical inner list in memory.

---

## 🧬 3. Deep Copy (`copy.deepcopy`)

A **Deep Copy** constructs a brand-new compound object and then **recursively copies all nested child objects** found within it. Modifications to any level of the clone have zero impact on the original:

```python
import copy

original = [[1, 2, 3], [4, 5, 6]]
deep = copy.deepcopy(original)

deep[0].append(999)

print("Original[0]:", original[0]) # [1, 2, 3]       (Protected!)
print("Deep[0]:    ", deep[0])     # [1, 2, 3, 999]   (Isolated clone)
```

---

## ⚖️ Summary Comparison

| Method | Syntax | Outer Object | Nested Child Objects |
| :--- | :--- | :--- | :--- |
| **Assignment** | `b = a` | Shared reference | Shared reference |
| **Shallow Copy** | `b = a.copy()` or `copy.copy(a)` | Independent clone | Shared references |
| **Deep Copy** | `b = copy.deepcopy(a)` | Independent clone | Independent recursive clones |

---

## Practice Quiz

### 1. In Python, what does the expression `b = a` do when `a` is a list?
A. It creates a complete duplicate copy of the list  
B. It creates a new reference `b` pointing to the exact same list in memory  
C. It converts the list into a tuple  
D. It deletes the original list  
**Answer:** B  
**Explanation:** Assignment in Python binds a new name to the existing object; it does not duplicate data.

---

### 2. Which module from Python's standard library provides shallow and deep copy utilities?
A. `clone`  
B. `sys`  
C. `copy`  
D. `types`  
**Answer:** C  
**Explanation:** Python's standard `copy` module contains `copy.copy()` and `copy.deepcopy()`.

---

### 3. What is the primary risk of using a shallow copy on a nested list?
A. It raises a `RecursionError`  
B. Modifying nested mutable elements inside the clone modifies the original nested data  
C. It consumes twice as much RAM as a deep copy  
D. It only works on strings  
**Answer:** B  
**Explanation:** A shallow copy only duplicates the outermost container; nested objects remain shared references.

---

### 4. Which function creates a completely independent clone of an object along with all its nested objects?
A. `copy.copy()`  
B. `copy.deepcopy()`  
C. `list.clone()`  
D. `object.duplicate()`  
**Answer:** B  
**Explanation:** `copy.deepcopy()` recursively traverses and duplicates all child and nested structures.

---

### 5. What does the `is` operator verify in `x is y`?
A. Whether `x` and `y` have identical values  
B. Whether `x` and `y` refer to the exact same object in memory (`id(x) == id(y)`)  
C. Whether `x` is an instance of `y`  
D. Whether `x` is greater than `y`  
**Answer:** B  
**Explanation:** The `is` operator checks object identity (memory address), whereas `==` checks value equality.

---

## 🚀 What's Next?

In the next lesson, we will implement our full OOP understanding in **Project: Bank Account Class System** (5: Object-Oriented Programming (Intermediate)).

👉 **[Continue to Next Lesson: Project: Bank Account Class System →](/tutorials/python-for-intermediate/project-bank-account-class-system)**
