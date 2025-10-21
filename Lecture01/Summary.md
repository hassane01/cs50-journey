
# 💻 CS50x 2025 – Lecture 1: C – Summary

---

## 🧠 What is C?
- 🏗️ **C Language:** A low-level programming language that gives direct control over hardware.
- 💡 **Why C?**
  - Foundation for many modern languages (Python, Java, etc.).
  - Helps understand how computers work “under the hood.”
- ⚙️ **Compiled Language:** Code must be converted into machine code before execution.

---

## 🔤 Writing Your First C Program
Example: `hello.c`
```c
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}
```
### Breakdown:
- `#include <stdio.h>` → includes standard input/output library.
- `int main(void)` → the main function where execution starts.
- `printf()` → prints formatted text to the screen.
- `\n` → newline character.

---

## 🧩 Compilation Process
1. **Preprocessing:** Handle `#include` and macros.
2. **Compiling:** Translate to assembly code.
3. **Assembling:** Create machine code.
4. **Linking:** Combine all pieces into an executable.

Command:
```bash
make hello
./hello
```

---

## 📦 Variables and Data Types
- 🧮 **Basic Types:**
  - `int` → integers
  - `float` → decimal numbers
  - `char` → single characters
  - `string` → sequence of chars (not built-in, from CS50 library)
  - `bool` → true/false (from `cs50.h`)
- 🧠 **Declaration Example:**
  ```c
  int age = 20;
  float price = 19.99;
  char letter = 'A';
  string name = "Hassane";
  ```

---

## 📥 Input and Output
Using the CS50 Library:
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name? ");
    printf("Hello, %s!\n", name);
}
```
- `get_string()` → gets user input.
- `%s` → string placeholder.

---

## 🔁 Conditionals and Logic
```c
if (x < y)
    printf("x is less than y\n");
else if (x > y)
    printf("x is greater than y\n");
else
    printf("x is equal to y\n");
```

### Logical Operators:
- `&&` → AND  
- `||` → OR  
- `!` → NOT  

---

## 🔂 Loops
### `for` loop:
```c
for (int i = 0; i < 3; i++)
{
    printf("meow\n");
}
```
### `while` loop:
```c
int i = 0;
while (i < 3)
{
    printf("meow\n");
    i++;
}
```

---

## 🧱 Functions
- Used to **organize and reuse code.**
```c
void meow(void)
{
    printf("meow\n");
}

int main(void)
{
    for (int i = 0; i < 3; i++)
        meow();
}
```
- `void` → means the function doesn’t return a value.

---

## 🔢 Command-Line Arguments
```c
int main(int argc, string argv[])
{
    printf("Hello, %s!\n", argv[1]);
}
```
- `argc` → argument count.
- `argv` → argument vector (list of strings).

---

## 🧮 Memory and Data Representation
- 💾 Each variable is stored in memory.
- `&x` → gives the **address** of variable `x`.
- `*` → dereference (get value at an address).

```c
int n = 50;
int *p = &n;
printf("%p\n", p);  // prints memory address
printf("%i\n", *p); // prints 50
```

---

## 🧠 Takeaways
- C builds the foundation of all programming logic.
- It teaches how memory, variables, and loops truly work.
- You now understand how high-level languages hide these details!

---

