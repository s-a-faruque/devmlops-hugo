+++
title = "Minimum Python Study Guide"
date = 2025-05-29
description = "A concise study guide for Python beginners and quick reference."
tags = ["python", "study-guide", "beginner"]
categories = ["programming", "python"]
author="Safique Ahmed Faruque"
image = "images/blog/python-minimum.png"
draft = false
+++

This is a quick-reference guide to Python for beginners and experienced developers who want a fast refresher.
<!--more-->
---

Here’s a cleaned-up and slightly enhanced version of your Python cheat sheet with a bit more clarity and structure for easier reference:

---

# **Python Data Types & Operations Cheat Sheet**

## **1. Data Types**

### **Primitive Types**

* `int` → integers (`5`, `-10`)
* `float` → floating-point numbers (`3.14`, `-0.5`)
* `str` → strings (`"hello"`)
* `bool` → boolean (`True`, `False`)

### **Complex Types**

* `list` → ordered, mutable collection (`[1, 2, 3]`)
* `tuple` → ordered, immutable collection (`(1, 2, 3)`)
* `dict` → key-value pairs (`{"a": 1, "b": 2}`)
* `set` → unordered, unique elements (`{1, 2, 3}`)

---

## **2. Collections: `list`, `dict`, `set` Operations**

### **Add Elements**

| Type   | Method                                                               |
| ------ | -------------------------------------------------------------------- |
| `list` | `append(item)` → add at end, `insert(index, item)` → add at position |
| `dict` | `dict[key] = value`                                                  |
| `set`  | `add(item)`                                                          |

### **Delete Elements**

| Type   | Method                                          |
| ------ | ----------------------------------------------- |
| `list` | `del list[index]`, `remove(item)`, `pop(index)` |
| `dict` | `pop(key)`                                      |
| `set`  | `remove(item)`                                  |

### **Access Elements**

* **Indexing:** `mylist[0]`
* **Slicing:** `mylist[start:end]`
* **Stepping:** `mylist[start:end:step]`

### **Search / Query**

* `list.index(item)` → get first occurrence index
* `list.count(item)` → count occurrences
* `item in list` → membership check

### **Other Useful Methods**

* `clear()` → remove all elements
* `len()` → length of collection
* `min()`, `max()` → minimum/maximum values

---

## **3. String Operations**

* `str.count(substring)` → count occurrences
* `str.find(substring)` → index of first occurrence (`-1` if not found)
* `str.strip()` → remove leading/trailing whitespace
* `str.split(delimiter)` → split string into a list

---

## **4. Math Operations**

* `min(iterable)` / `max(iterable)`
* `pow(x, y)` → x to the power y
* `abs(x)` → absolute value
* `math.floor(x)` → largest integer ≤ x (**requires `import math`**)

---

## Conditions

### If / Else Syntax

```python
if condition1:
    # do something
elif condition2:
    # do something else
else:
    # fallback action
```

### Ternary Operator

```python
status = "Adult" if age >= 18 else "Minor"
```

#### Equivalent in Other Languages

```js
status = age >= 18 ? "Adult" : "Minor";
```

---

## Loops

### Common For-Loop Patterns

```python
# Iterate over values
for value in values:
    ...

# Index and value
for index, value in enumerate(values):
    ...

# Keys only
for key in my_dict:
    ...

# Keys and values
for key, value in my_dict.items():
    ...

# Range loop
for i in range(5):
    ...
```

---

## Object-Oriented Programming (OOP)

### Class Definition

```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hello, {self.name}"
```

---

This guide covers the essentials. For deeper learning, explore Python's [official documentation](https://docs.python.org/3/), or practice with small projects and challenges.
