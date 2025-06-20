+++
title = "Minimum Python Study Guide"
date = 2025-05-29
description = "A concise study guide for Python beginners and quick reference."
tags = ["python", "study-guide", "beginner"]
categories = ["programming", "python"]
author="Safique Faruque"
+++

This is a quick-reference guide to Python for beginners and experienced developers who want a fast refresher.
<!--more-->
---

## Data Types

### Primitive Types

* `int`
* `float`
* `str`
* `bool`

### Complex Types

* `list`
* `tuple`
* `dict`
* `set`

---

## Collections: `list`, `dict`, `set` Operations

### Add

* `list.append(item)`
* `list.insert(index, item)`
* `dict[key] = value`
* `set.add(item)`

### Delete

* `del list[index]`
* `list.remove(item)`
* `list.pop(index)`
* `dict.pop(key)`
* `set.remove(item)`

### Access

* Indexing: `mylist[0]`
* Slicing: `mylist[start:end]`
* Stepping: `mylist[start:end:step]`

### Search

* `list.index(item)`
* `list.count(item)`
* `item in list`

### Other

* `clear()`
* `len()`
* `min()`
* `max()`

---

## String Operations

* `str.count(substring)`
* `str.find(substring)`
* `str.strip()`
* `str.split(delimiter)`

---

## Math Operations

* `min()`
* `max()`
* `pow(x, y)`
* `abs(x)`
* `math.floor(x)` *(requires `import math`)*

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

Let me know if you want a version in Markdown without TOML front matter, or a downloadable version (PDF or HTML).
