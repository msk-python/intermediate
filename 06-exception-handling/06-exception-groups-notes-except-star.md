---
id: python-intermediate-exception-groups-notes-except-star
slug: exception-groups-notes-except-star
course: python-for-intermediate
chapter: "6: Exception Handling"
topic: "6.6 Modern Exception Handling: Exception Groups, except*, and add_note()"
title: "Modern Exception Handling in Python: Exception Groups & except*"
description: "Master modern Python 3.11+ exception handling: ExceptionGroup, selective handling with except*, enriching tracebacks with add_note(), and interactive quiz."
difficulty: Intermediate
readingTime: 10
order: 32
keywords:
  - python exceptiongroup
  - python except star
  - python add_note
  - python 3.11 exception handling
  - pep 654 python
lastUpdated: 2026-10-23
author: MSK Institute
version: 1.0.0
---

# 🐍 Modern Exception Handling: Exception Groups & except*

Traditional Python exception handling assumed that errors happen **one at a time**: a function executes, a single failure occurs, an exception is raised, and a matching `except` block catches it.

However, in modern software architectures (concurrent tasks, async event loops, parallel batch processing), **multiple independent errors can occur simultaneously**.

To address this, **Python 3.11** introduced **Exception Groups** (PEP 654) and **Exception Notes** (PEP 678), transforming how enterprise Python applications handle failures.

---

## 📦 1. What is an `ExceptionGroup`?

An `ExceptionGroup` bundles multiple exception instances into a single composite object that can be raised together:

```python
# Raising multiple errors simultaneously:
errors = [
    ValueError("Invalid port number: 99999"),
    FileNotFoundError("Missing config.json file"),
    ConnectionRefusedError("Database host unreachable on 127.0.0.1:5432")
]

# Syntax: ExceptionGroup(message, list_of_exceptions)
raise ExceptionGroup("Server initialization failed with multiple errors", errors)
```

---

## ⭐ 2. Selective Handling with `except*`

Standard `try...except` blocks cannot unpack and selectively catch individual error types inside an `ExceptionGroup`. 

Python 3.11 introduced the **`except*`** syntax (pronounced *"except star"*):
- Each `except*` clause inspects the group and extracts matching exceptions.
- **Multiple `except*` branches can execute** if the group contains different matching exception types!

```python
try:
    raise ExceptionGroup("Batch Job Errors", [
        ValueError("Negative student age"),
        TypeError("Expected string name, received int"),
        ValueError("Roll number cannot be zero")
    ])
except* ValueError as eg:
    print(f"Handled {len(eg.exceptions)} ValueErrors:")
    for err in eg.exceptions:
        print(f"  -> {err}")
except* TypeError as eg:
    print(f"Handled TypeError: {eg.exceptions[0]}")
```

### Output:
```text
Handled 2 ValueErrors:
  -> Negative student age
  -> Roll number cannot be zero
Handled TypeError: Expected string name, received int
```

---

## 📝 3. Enriching Tracebacks with `add_note()` (PEP 678)

Have you ever caught an exception and wished you could attach diagnostic context (like user ID, request payload, or file path) without altering the exception type?

Starting in Python 3.11, all standard exceptions have an **`add_note()`** method:

```python
def process_student_enrollment(student_id: int):
    try:
        raise ConnectionError("Payment gateway timed out.")
    except ConnectionError as err:
        err.add_note(f"Context: Processing student ID: {student_id}")
        err.add_note("Gateway: Razorpay Production API Endpoint")
        raise # Re-raises exception with notes visible in traceback!
```

When printed or logged, Python displays these helpful diagnostic notes directly in the error traceback!

---

## Practice Quiz

### 1. In which Python version were Exception Groups (`ExceptionGroup` and `except*`) introduced?
A. Python 3.8  
B. Python 3.10  
C. Python 3.11  
D. Python 3.13  
**Answer:** C  
**Explanation:** PEP 654 introduced `ExceptionGroup` and `except*` in Python 3.11.

---

### 2. Can multiple `except*` clauses execute for a single `try` block?
A. No, only the first matching clause runs, identical to regular `except`  
B. Yes, different `except*` clauses can execute if the `ExceptionGroup` contains multiple matching error types  
C. Only if configured with a threading lock  
D. No, `except*` causes a syntax error in Python  
**Answer:** B  
**Explanation:** `except*` matches partitions of the exception group; if a group contains both a `ValueError` and a `TypeError`, both respective `except*` handlers execute.

---

### 3. What method allows you to attach custom context strings to any Python 3.11+ exception?
A. `err.attach_context()`  
B. `err.add_note()`  
C. `err.set_comment()`  
D. `err.append_message()`  
**Answer:** B  
**Explanation:** PEP 678 added the `add_note(string)` method to `BaseException`.

---

### 4. What happens to unhandled exceptions inside an `ExceptionGroup` if only some types are caught by `except*`?
A. They are silently swallowed and ignored  
B. The remaining unmatched exceptions are repackaged into a sub-group and automatically re-raised  
C. The system crashes with an exit code 1 immediately  
D. They are converted into warnings  
**Answer:** B  
**Explanation:** Python re-raises any unmatched exceptions in a child ExceptionGroup after all applicable `except*` clauses finish.

---

### 5. Can you mix regular `except` and `except*` in the same `try` block?
A. Yes, freely  
B. No, mixing `except` and `except*` in the same `try` statement is a SyntaxError  
C. Only if `except` comes first  
D. Only inside classes  
**Answer:** B  
**Explanation:** Python's grammar strictly forbids mixing traditional `except` and modern `except*` within the same compound statement.

---

## 🚀 What's Next?

In the next lesson, we will put our robust exception handling skills to work in **Project: Error-Handled Calculator** (6: Exception Handling).

👉 **[Continue to Next Lesson: Project: Error-Handled Calculator →](/tutorials/python-for-intermediate/project-error-handled-calculator)**
