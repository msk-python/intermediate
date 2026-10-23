---
id: python-intermediate-dataclasses-module
slug: dataclasses-module
course: python-for-intermediate
chapter: "5: Object-Oriented Programming (Intermediate)"
topic: "5.6 Dataclasses: Clean Data Models with @dataclass"
title: "Dataclasses in Python: Clean Data Models with @dataclass"
description: "Master Python dataclasses: automatic __init__, __repr__, and __eq__, default factories, frozen immutable classes, kw_only, and interactive quiz."
difficulty: Intermediate
readingTime: 10
order: 26
keywords:
  - python dataclass
  - dataclasses module
  - pep 557 python
  - clean python oop
  - frozen dataclass
lastUpdated: 2026-10-23
author: MSK Institute
version: 1.0.0
---

# 🐍 Dataclasses: Clean Data Models with @dataclass

When writing object-oriented Python to store data, developers historically wrote immense amounts of repetitive boilerplate:
- Defining `__init__()` assigning `self.x = x`
- Defining `__repr__()` to make printing readable
- Defining `__eq__()` so instances could be compared for equality
- Defining `__hash__()` for dictionary keys

Python 3.7 introduced the standard **`dataclasses`** module (PEP 557). It automatically generates these special dunder methods using class type annotations!

---

## 🧱 1. Traditional Class vs @dataclass

### The Boilerplate Heavy Way:
```python
class Student:
    def __init__(self, name: str, roll_no: int, course: str = "Python Mastery"):
        self.name = name
        self.roll_no = roll_no
        self.course = course

    def __repr__(self):
        return f"Student(name={self.name!r}, roll_no={self.roll_no}, course={self.course!r})"

    def __eq__(self, other):
        if not isinstance(other, Student):
            return False
        return (self.name, self.roll_no, self.course) == (other.name, other.roll_no, other.course)
```

### The Modern `@dataclass` Way:
```python
from dataclasses import dataclass

@dataclass
class Student:
    name: str
    roll_no: int
    course: str = "Python Mastery"

s1 = Student("Aarav Sharma", 101)
s2 = Student("Aarav Sharma", 101)

print(s1)          # Student(name='Aarav Sharma', roll_no=101, course='Python Mastery')
print(s1 == s2)    # True! (__eq__ generated automatically)
```

Just 5 lines of code replaced 18 lines of boilerplate!

---

## 🎛️ 2. Default Values & `field(default_factory=...)`

Never set mutable defaults (like lists or dictionaries) directly in class attributes. Use `field(default_factory=...)`:

```python
from dataclasses import dataclass, field

@dataclass
class StudentBatch:
    batch_name: str
    students: list[str] = field(default_factory=list)

morning_batch = StudentBatch("Morning Python Cohort")
morning_batch.students.append("Aarav")
morning_batch.students.append("Diya")

print(morning_batch)
# StudentBatch(batch_name='Morning Python Cohort', students=['Aarav', 'Diya'])
```

---

## ❄️ 3. Immutable Data Models with `frozen=True`

If you want your data structure to be read-only (immutable like a tuple) and hashable so it can be used as a dictionary key or placed inside a `set`, pass `frozen=True`:

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class CoursePricing:
    course_slug: str
    tuition_fee: int

fee_tier = CoursePricing("python-mastery", 4999)
print(fee_tier.tuition_fee)

# ❌ FrozenInstanceError: cannot assign to field 'tuition_fee'
# fee_tier.tuition_fee = 3999
```

---

## ⚙️ 4. Post-Initialization with `__post_init__()`

If you need to validate data or compute derived fields immediately after `__init__` runs, define `__post_init__()`:

```python
from dataclasses import dataclass

@dataclass
class Employee:
    name: str
    base_salary: float
    bonus: float = 0.0
    total_comp: float = field(init=False)

    def __post_init__(self):
        self.total_comp = self.base_salary + self.bonus

emp = Employee("Er. Sumit Kumar", 150000.0, 25000.0)
print(f"Total Compensation: ₹{emp.total_comp:,.2f}") # ₹175,000.00
```

---

## Practice Quiz

### 1. In which Python version was the `dataclasses` module officially introduced?
A. Python 3.5  
B. Python 3.7  
C. Python 3.10  
D. Python 3.12  
**Answer:** B  
**Explanation:** `dataclasses` was introduced in Python 3.7 under PEP 557.

---

### 2. Which dunder methods does `@dataclass` generate by default?
A. `__init__`, `__repr__`, and `__eq__`  
B. Only `__str__`  
C. Only `__hash__`  
D. `__enter__` and `__exit__`  
**Answer:** A  
**Explanation:** By default, `@dataclass` inspects type annotations and synthesizes `__init__`, `__repr__`, and `__eq__`.

---

### 3. Why should you use `field(default_factory=list)` instead of `students: list = []` in a dataclass?
A. Python does not allow square brackets in dataclasses  
B. Mutable default arguments are shared across all instances, causing accidental shared state  
C. `default_factory` speeds up execution by 50%  
D. It is required only for numbers  
**Answer:** B  
**Explanation:** Setting a mutable container directly as a default value causes every instance to share that exact same list object in memory.

---

### 4. What does the `frozen=True` parameter enforce on a dataclass?
A. It compiles the class to C code  
B. Instances become immutable; attempting to modify an attribute raises a `FrozenInstanceError`  
C. The class cannot be inherited from  
D. Attributes cannot have type hints  
**Answer:** B  
**Explanation:** `frozen=True` makes instances read-only and generates a valid `__hash__` implementation.

---

### 5. What is the purpose of the `__post_init__()` method in a dataclass?
A. It runs before Python creates the object  
B. It executes immediately after the generated `__init__()` completes, allowing validation and derived attribute computation  
C. It deletes the instance from memory  
D. It converts the dataclass into a JSON string  
**Answer:** B  
**Explanation:** `__post_init__` provides a hook to perform custom validation or calculate dependent attributes right after initialization.

---

## 🚀 What's Next?

In the next lesson, we will master **Object Mutability: Shallow Copy vs Deep Copy** (5: Object-Oriented Programming (Intermediate)).

👉 **[Continue to Next Lesson: Object Mutability: Shallow Copy vs Deep Copy →](/tutorials/python-for-intermediate/shallow-vs-deep-copy)**
