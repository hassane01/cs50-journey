# 🐍 CS50 Lecture 6: Python

> **Course**: CS50 - Harvard University's Introduction to Computer Science  
> **Lecture**: Week 6 - Python  
> **Focus**: Transition from low-level C to high-level Python programming

---

## 🎯 Learning Objectives

By the end of this lecture, you will understand:
- Python as a higher-level, interpreted language
- Key syntax differences between Python and C
- Dynamic typing and automatic memory management
- Object-Oriented Programming concepts
- Python's built-in data structures and libraries
- Exception handling and error management
- File I/O and CSV operations
- Package management with pip

---

## 📑 Table of Contents

1. [Introduction to Python](#-introduction-to-python)
2. [Functions and Basic Syntax](#-functions-and-basic-syntax)  
3. [Data Types and Variables](#-data-types-and-variables)
4. [Conditionals](#-conditionals)
5. [Object-Oriented Programming](#-object-oriented-programming)
6. [Loops](#-loops)
7. [Exception Handling](#-exception-handling)
8. [Data Structures](#-data-structures)
9. [File Operations](#-file-operations)
10. [Libraries and Modules](#-libraries-and-modules)

---

## 🚀 Introduction to Python

**Python** is a **high-level, interpreted programming language** that emphasizes code readability and simplicity.

### 🔄 C vs Python Comparison

| Feature | C (Compiled) | Python (Interpreted) |
|---------|-------------|---------------------|
| **Memory Management** | Manual (`malloc`, `free`) | Automatic |
| **Type Declaration** | Required (`int x;`) | Dynamic (`x = 5`) |
| **Compilation** | Two steps: compile → run | One step: interpret |
| **Syntax** | Verbose, explicit | Concise, readable |
| **Hello World** | 6+ lines | 1 line |

### ⚡ Quick Demonstration

**Hello World Evolution:**

**Scratch (Week 0):**
```
say "hello, world"
```

**C (Week 1):**
```c
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}
```

**Python (Week 6):**
```python
print("hello, world")
```

### 🎨 Powerful Libraries in Action

**Image Processing** (4 lines vs hundreds in C):
```python
from PIL import Image, ImageFilter

before = Image.open("bridge.bmp")
after = before.filter(ImageFilter.BoxBlur(10))
after.save("out.bmp")
```

**Problem Set 5 Spell Checker** (Entire dictionary implementation):
```python
words = set()

def check(word):
    return word.lower() in words

def load(dictionary):
    with open(dictionary) as file:
        words.update(file.read().splitlines())
    return True

def size():
    return len(words)

def unload():
    return True  # Python handles memory automatically!
```

---

## 📝 Functions and Basic Syntax

### 🔤 Print Function Evolution

| Language | Syntax | Notes |
|----------|--------|-------|
| **C** | `printf("hello, world\n");` | Requires `\n` for newline |
| **Python** | `print("hello, world")` | Newline automatic, no semicolon |

### 📥 Getting User Input

**Three Ways to Handle User Input:**

#### 1️⃣ String Concatenation
```python
from cs50 import get_string

answer = get_string("What's your name? ")
print("hello, " + answer)
```

#### 2️⃣ Comma-Separated Arguments
```python
from cs50 import get_string

answer = get_string("What's your name? ")
print("hello,", answer)  # Automatic space insertion
```

#### 3️⃣ F-Strings (Most Popular) ⭐
```python
from cs50 import get_string

answer = get_string("What's your name? ")
print(f"hello, {answer}")
```

**🔑 Key Concept**: The `f` prefix creates a **format string** where `{variable}` gets replaced with the variable's value.

### 🛠️ Named Parameters

Python functions support **named parameters** alongside **positional parameters**.

**Print Function Signature:**
```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

**Customizing Output:**
```python
print("hello, world", end="!\n")  # Custom ending
print("A", "B", "C", sep="-")     # Custom separator: A-B-C
```

### 💡 Import Styles

**Two common import patterns:**

```python
# Method 1: Import entire module
import cs50
name = cs50.get_string("Name: ")

# Method 2: Import specific functions
from cs50 import get_string
name = get_string("Name: ")
```

---

## 🔢 Data Types and Variables

### 📊 Python Data Types

Python uses **dynamic typing** - no need to declare variable types!

#### Primitive Types
```python
# C: int counter = 0;
counter = 0        # Python automatically infers int

# C: char name[50];
name = "David"     # Python automatically infers str

# C: float price = 9.99;
price = 9.99       # Python automatically infers float

# C: bool flag = true;
flag = True        # Note: Capital T and F
```

#### Built-in Data Types
| Type | Python Name | Notes |
|------|-------------|-------|
| **Boolean** | `bool` | `True`, `False` (capitalized) |
| **Integer** | `int` | Arbitrary precision (no overflow!) |
| **Float** | `float` | Double precision by default |
| **String** | `str` | No separate `char` type |

#### Built-in Data Structures
| Structure | Purpose | Example |
|-----------|---------|---------|
| **`list`** | Dynamic arrays | `[1, 2, 3]` |
| **`dict`** | Key-value pairs | `{"name": "David"}` |
| **`set`** | Unique values | `{1, 2, 3}` |
| **`tuple`** | Immutable sequences | `(x, y)` |

### 🔄 Variable Operations

**Incrementing (C vs Python):**

| Operation | C | Python | Supported? |
|-----------|---|--------|-----------|
| Assignment | `counter = counter + 1;` | `counter = counter + 1` | ✅ |
| Compound | `counter += 1;` | `counter += 1` | ✅ |
| Increment | `counter++;` | `counter++` | ❌ |

### 🧮 Calculator Example

**Type Conversion Demonstration:**

```python
# Problem: input() returns strings
x = input("x: ")  # User types "1"
y = input("y: ")  # User types "2"
print(x + y)      # Output: "12" (concatenation!)

# Solution: Type conversion
x = int(input("x: "))
y = int(input("y: "))
print(x + y)      # Output: 3 (addition!)
```

**🎯 Key Insight**: Python's `+` operator is **overloaded** - works differently for strings (concatenation) vs numbers (addition).

---

## 🔀 Conditionals

### 📋 Syntax Comparison

**Major Differences from C:**
- ❌ No parentheses around conditions
- ✅ Colon `:` after condition
- ✅ **Indentation matters** (replaces curly braces)
- ✅ `elif` instead of `else if`

#### If Statements
```python
# C Style
if (x < y) {
    printf("x is less than y\n");
}

# Python Style  
if x < y:
    print("x is less than y")
```

#### If-Elif-Else Chain
```python
x = int(input("What's x? "))
y = int(input("What's y? "))

if x < y:
    print("x is less than y")
elif x > y:
    print("x is greater than y")
else:
    print("x is equal to y")
```

### 🔤 String Comparison

**Massive Improvement over C:**

```python
# C (complicated)
char *s = get_string("s: ");
char *t = get_string("t: ");
if (strcmp(s, t) == 0) { ... }

# Python (simple!)
s = input("s: ")
t = input("t: ")
if s == t:
    print("Same")
else:
    print("Different")
```

### 🔗 Logical Operators

| C Operator | Python Operator | Example |
|-----------|----------------|---------|
| `\|\|` | `or` | `if s == "Y" or s == "y":` |
| `&&` | `and` | `if age >= 18 and has_license:` |
| `!` | `not` | `if not is_valid:` |

### 🎯 The `in` Operator

**Elegant membership testing:**

```python
# Old way (verbose)
s = input("Do you agree? ")
if s == "Y" or s == "y" or s == "yes" or s == "Yes":
    print("Agreed")

# Python way (elegant)
s = input("Do you agree? ")
if s in ["Y", "y", "yes", "Yes"]:
    print("Agreed")
```

---

## 🎭 Object-Oriented Programming

### 🏗️ Core Concepts

**Object-Oriented Programming (OOP)** treats data types as **objects** with associated **methods** (functions).

#### 📦 Objects = Data + Methods

```python
name = "david"      # name is a string object
name.upper()        # .upper() is a method of string objects
name.lower()        # .lower() is a method of string objects
name.capitalize()   # .capitalize() is a method of string objects
```

### 🔤 String Methods

**Common String Methods:**

| Method | Purpose | Example |
|--------|---------|---------|
| `.lower()` | Convert to lowercase | `"HELLO".lower()` → `"hello"` |
| `.upper()` | Convert to uppercase | `"hello".upper()` → `"HELLO"` |
| `.capitalize()` | Capitalize first letter | `"hello".capitalize()` → `"Hello"` |
| `.isnumeric()` | Check if all digits | `"123".isnumeric()` → `True` |

### ⛓️ Method Chaining

**Powerful technique for data transformation:**

```python
# Individual steps
user_input = input("Do you agree? ")
lowercased = user_input.lower()
if lowercased in ["y", "yes"]:
    print("Agreed")

# Chained (more elegant)
if input("Do you agree? ").lower() in ["y", "yes"]:
    print("Agreed")
```

---

## 🔄 Loops

### 🔁 While Loops

**Syntax remains similar to C:**

```python
# C style
while (i < 3) {
    printf("meow\n");
    i++;
}

# Python style
i = 0
while i < 3:
    print("meow")
    i += 1
```

### 🔢 For Loops

**Python's `for` loops are fundamentally different and more powerful than C:**

#### Traditional C-Style Loop (Avoid)
```python
# Don't do this in Python!
i = 0
while i < 3:
    print("meow")
    i += 1
```

#### Pythonic For Loop with Range
```python
# Better: Use range()
for i in range(3):    # 0, 1, 2
    print("meow")

# If you don't use the loop variable
for _ in range(3):    # _ indicates unused variable
    print("meow")
```

#### For-Each Style Loop
```python
# Loop over collection elements directly
names = ["Alice", "Bob", "Charlie"]
for name in names:
    print(f"Hello, {name}")

# Loop over string characters
word = "hello"
for char in word:
    print(char.upper(), end="")
```

### 🏗️ Defining Functions

**Python functions use `def` keyword:**

```python
def meow():
    print("meow")

def meow_n_times(n):
    for _ in range(n):
        print("meow")

# Function calls
meow()
meow_n_times(3)
```

### 📋 Function Organization

**Best Practice - Use `main()` function:**

```python
def main():
    number = int(input("Number: "))
    meow(number)

def meow(n):
    for _ in range(n):
        print("meow")

# Call main at the end
main()
```

**🎯 Why?** Functions must be **defined before** they're called in Python (unlike C with prototypes).

### 🔄 Alternative Entry Point

**Professional Python convention:**

```python
def main():
    meow(3)

def meow(n):
    for _ in range(n):
        print("meow")

if __name__ == "__main__":
    main()
```

**📚 Explanation**: This allows the file to work as both a script and a module.

---

## ⚠️ Exception Handling

### 🚨 The Problem with Traditional Error Handling

**C approach (sentinel values):**
```c
int n = get_int("Number: ");  // Returns some error value for invalid input
if (n == /* some error value */) {
    // Handle error
}
```

**❌ Problems:**
- What if the error value is also a valid input?
- Inconsistent error signaling across functions

### 🛡️ Python's Exception System

**Exceptions** are Python's elegant way to handle errors:

```python
# This will crash with ValueError if input is invalid
n = int(input("Number: "))
print("Integer")
```

### 🎯 Try-Except Blocks

**Basic exception handling:**

```python
try:
    n = int(input("Number: "))
    print("Integer")
except ValueError:
    print("Not integer")
```

### 🎭 Try-Except-Else Pattern

**Better style with `else` block:**

```python
try:
    n = int(input("Number: "))
except ValueError:
    print("Not integer")
else:
    print("Integer")  # Only runs if no exception
```

**🔑 Key Concept**: `else` block only executes if the `try` block completes **without** an exception.

### 🔄 How CS50's `get_int()` Works

**Behind the scenes:**

```python
def get_int(prompt):
    while True:
        try:
            return int(input(prompt))
        except ValueError:
            print("Invalid input. Try again.")
            # Loop continues
```

### 🏷️ Common Exception Types

| Exception | Cause | Example |
|-----------|-------|---------|
| `ValueError` | Invalid type conversion | `int("hello")` |
| `NameError` | Undefined variable | Using undefined `x` |
| `KeyError` | Invalid dictionary key | `dict["missing_key"]` |
| `IndexError` | Invalid list index | `list[999]` |
| `KeyboardInterrupt` | Ctrl+C pressed | User interruption |

---

## 📊 Data Structures

### 📝 Lists

**Dynamic, resizable arrays:**

#### Creating and Using Lists
```python
# Declaration with values
scores = [72, 73, 33]

# Empty list
scores = []

# Adding elements
scores.append(85)
scores.append(92)

# Alternative: concatenation
scores = scores + [78]
scores += [90]
```

#### Built-in Functions
```python
scores = [72, 73, 33]

length = len(scores)      # 3
total = sum(scores)       # 178
average = sum(scores) / len(scores)  # 59.33

print(f"Average: {average}")
```

#### Membership Testing
```python
names = ["Alice", "Bob", "Charlie"]
name = input("Name: ")

if name in names:
    print("Found")
else:
    print("Not found")
```

#### For Loop with Else
```python
names = ["Alice", "Bob", "Charlie"]
name = input("Name: ")

for n in names:
    if n == name:
        print("Found")
        break
else:  # Only executes if loop completes without break
    print("Not found")
```

### 🗂️ Dictionaries

**Key-value pairs (hash tables):**

#### List of Dictionaries
```python
people = [
    {"name": "Alice", "number": "+1-617-495-1000"},
    {"name": "Bob", "number": "+1-617-495-1000"},
    {"name": "Charlie", "number": "+1-949-468-2750"}
]

# Searching
name = input("Name: ")
for person in people:
    if person["name"] == name:
        print(f"Found: {person['number']}")
        break
else:
    print("Not found")
```

#### Direct Dictionary (More Efficient)
```python
people = {
    "Alice": "+1-617-495-1000",
    "Bob": "+1-617-495-1000", 
    "Charlie": "+1-949-468-2750"
}

# Searching (much simpler!)
name = input("Name: ")
if name in people:
    print(f"Found: {people[name]}")
else:
    print("Not found")
```

**🎯 Key Advantages of Dictionaries:**
- ⚡ O(1) average lookup time
- 🎯 Direct key access
- 🧹 Cleaner, more readable code

---

## 📁 File Operations

### 💻 Command Line Arguments

**Using `sys.argv` (similar to C's argv):**

```python
import sys

# sys.argv[0] is script name
# sys.argv[1] is first argument, etc.

if len(sys.argv) == 2:
    print(f"Hello, {sys.argv[1]}")
else:
    print("Hello, world")
```

### 🚪 Exit Status

**Explicit exit codes:**

```python
import sys

if len(sys.argv) != 2:
    print("Missing command-line argument")
    sys.exit(1)  # Exit with error status
else:
    print(f"Hello, {sys.argv[1]}")
    sys.exit(0)  # Exit with success status
```

**💡 Check exit status in terminal:** `echo $?`

### 📄 CSV File Operations

**Reading and writing structured data:**

#### Writing CSV Files
```python
import csv

name = input("Name: ")
number = input("Number: ")

# Method 1: Basic writer
with open("phonebook.csv", "a") as file:
    writer = csv.writer(file)
    writer.writerow([name, number])

# Method 2: Dictionary writer (better for complex data)
with open("phonebook.csv", "a") as file:
    writer = csv.DictWriter(file, fieldnames=["name", "number"])
    writer.writerow({"name": name, "number": number})
```

#### Reading CSV Files
```python
import csv

with open("phonebook.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(f"Name: {row[0]}, Number: {row[1]}")

# Dictionary reader
with open("phonebook.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(f"Name: {row['name']}, Number: {row['number']}")
```

**🔑 `with` Statement**: **Context manager** that automatically closes files, even if errors occur.

---

## 📚 Libraries and Modules

### 🐍 Python Package Index (PyPI)

**`pip` - The Package Installer for Python:**

```bash
pip install package_name
```

### 🎭 Fun Examples

#### Cowsay
```python
# Installation: pip install cowsay

import cowsay

name = input("What's your name? ")
cowsay.cow(f"Hello, {name}")
```

#### QR Code Generator
```python
# Installation: pip install qrcode

import qrcode

img = qrcode.make("https://youtu.be/xvFZjo5PgG0")
img.save("qr.png")
```

### 🔧 Practical Libraries

| Library | Purpose | Installation |
|---------|---------|-------------|
| **PIL/Pillow** | Image processing | `pip install Pillow` |
| **pandas** | Data analysis | `pip install pandas` |
| **requests** | HTTP requests | `pip install requests` |
| **numpy** | Numerical computing | `pip install numpy` |
| **matplotlib** | Data visualization | `pip install matplotlib` |

---

## 🎯 Key Takeaways

### 1️⃣ **Python's Philosophy: Simplicity**

**The Zen of Python:**
- Beautiful is better than ugly
- Simple is better than complex
- Readable counts
- There should be one obvious way to do it

### 2️⃣ **Major Advantages of Python**

✅ **Automatic Memory Management** - No `malloc`/`free`  
✅ **Dynamic Typing** - Variables adapt automatically  
✅ **Rich Standard Library** - "Batteries included"  
✅ **Readable Syntax** - Code that looks like English  
✅ **Rapid Development** - Focus on problem-solving, not boilerplate  

### 3️⃣ **Language Transition Skills**

**🎓 Learning New Languages:**
- Recognize **patterns** and **similarities**
- Understand **fundamental concepts** transfer between languages
- **Google syntax**, focus on **logic**
- Use **documentation** effectively

### 4️⃣ **When to Use Python vs C**

| Use Case | Recommended Language | Why |
|----------|-------------------|-----|
| **System Programming** | C | Direct hardware control, performance |
| **Web Development** | Python | Rich frameworks, rapid development |
| **Data Science** | Python | Excellent libraries (pandas, numpy) |
| **Embedded Systems** | C | Memory constraints, real-time requirements |
| **Scripting/Automation** | Python | Quick development, excellent string handling |

---

## ⚡ Performance Considerations

### 🏎️ Speed vs Development Time

**Trade-off Analysis:**

| Factor | C | Python |
|--------|---|--------|
| **Execution Speed** | ⚡⚡⚡ Very Fast | ⚡⚡ Moderate |
| **Development Speed** | 🐌 Slow | 🚀 Very Fast |
| **Memory Usage** | 💾 Minimal | 💾💾 Higher |
| **Learning Curve** | 📈 Steep | 📈 Gentle |

### 🧮 Floating Point Precision

**Still an issue in Python:**

```python
result = 1 / 3
print(f"{result:.50f}")  # Shows imprecision beyond 15-16 digits
```

**💡 Solution**: Use `decimal` module for high-precision arithmetic when needed.

### 🔢 Integer Overflow

**Not a problem in Python!**

```python
# This works in Python, would overflow in C
big_number = 2 ** 1000
print(big_number)  # Prints huge number successfully
```

---

## 🎬 Looking Ahead

### Week 7: SQL and Databases

Python makes database operations much easier:

```python
import sqlite3

conn = sqlite3.connect('database.db')
cursor = conn.cursor()
cursor.execute("SELECT * FROM students WHERE name = ?", (name,))
```

### Week 8: Web Programming

Python web frameworks like Flask:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return "Hello, World!"
```

---

## 🎓 Practice Exercises

### 🔥 Mario Pyramid

**Vertical:**
```python
from cs50 import get_int

while True:
    height = get_int("Height: ")
    if height > 0:
        break

for i in range(height):
    print("#")
```

**Horizontal:**
```python
from cs50 import get_int

while True:
    height = get_int("Height: ")
    if height > 0:
        break

for i in range(height):
    print("#" * (i + 1))
```

### 📞 Phonebook Search

```python
people = {
    "Alice": "+1-617-495-1000",
    "Bob": "+1-617-495-1001",
    "Charlie": "+1-617-495-1002"
}

name = input("Name: ")
if name in people:
    print(f"Found: {people[name]}")
else:
    print("Not found")
```

---

## 📝 Common Pitfalls and Solutions

### ❌ Common Mistakes

1️⃣ **Forgetting the `f` in f-strings:**
```python
name = "David"
print("Hello, {name}")  # Wrong: prints literal braces
print(f"Hello, {name}") # Correct: Hello, David
```

2️⃣ **Inconsistent indentation:**
```python
if x > 0:
    print("Positive")
  print("This line")  # IndentationError!
```

3️⃣ **String vs integer addition:**
```python
x = input("x: ")  # Returns string "5"
y = input("y: ")  # Returns string "3"
print(x + y)      # "53", not 8!
```

### ✅ Best Practices

1️⃣ **Use meaningful variable names**
2️⃣ **Be consistent with quotes** (prefer double quotes)
3️⃣ **Use `main()` function for organization**
4️⃣ **Handle exceptions appropriately**
5️⃣ **Use f-strings for formatting**

---

## 🌟 Final Thoughts

**Python represents a philosophical shift:**
- From **"How do I make the computer do this?"**
- To **"What do I want to accomplish?"**

The language handles the tedious details so you can focus on **solving interesting problems**.

**🎯 Remember**: The goal isn't to memorize every syntax detail, but to understand **programming concepts** that transfer between languages. Python just makes those concepts easier to express and implement.

---

## 📚 Additional Resources

- **Official Python Documentation**: [docs.python.org](https://docs.python.org)
- **Python Package Index (PyPI)**: [pypi.org](https://pypi.org)
- **CS50 Python Library**: [pypi.org/project/cs50](https://pypi.org/project/cs50)
- **Python Style Guide (PEP 8)**: [pep8.org](https://pep8.org)

---

## 🎬 Lecture Credits

**Instructor**: David J. Malan  
**University**: Harvard University  
**Course**: CS50x 2025  
**Video**: CS50x 2025 - Lecture 6 - Python

---

*Made with 🐍 for CS50 students transitioning from C to Python*
