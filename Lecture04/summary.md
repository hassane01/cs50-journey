

# Stylish README content for Lecture 4
readme_content = """
# 📘 CS50x 2025 – Lecture 4: Memory – Summary

---

## 🧠 Introduction: The Reality of Memory
- 💦 **Challenging Topic:** Memory and pointers are notoriously tricky but foundational.
- 📸 **Motivation via Images:** Represent pixels as data, simplest images 1-bit, color images 24-bit (RGB).
- 🔢 **Hexadecimal Notation:** Base 16 (0-9, A-F), used for colors and memory addresses.

---

## 📍 Pointers: Addresses in Memory
- 🏷️ **Variables Have Addresses:** Each variable stored at a memory location (hexadecimal like 0x7ff...).
- 🆕 **C Syntax:**
  - `&` → address-of operator
  - `*` → dereference operator
  - `%p` → printf format for pointer
- 💡 **Declaring a Pointer:** `int *p = &n;`
- 📬 **Mailbox Analogy:** Pointer stores address of another variable; dereference to access value.

---

## 🔤 The True Nature of Strings
- `"string"` doesn’t exist in C; use `char *` instead.
- **char *** stores the address of the first character.
- **Null terminator (\0):** marks end of string.
- **Pointer Arithmetic:** `s + i` or `*(s + i)`; `s[i]` is syntactic sugar.

---

## 🧩 Memory Layout and Common Pitfalls
- **Stack vs Heap:**
  - Stack: local variables, grows/shrinks with function calls.
  - Heap: dynamic memory (malloc), not size-known at compile time.
- **Passing by Value vs Reference:** 
  - By default: pass by value → copy of variable.
  - To modify original: pass pointer (reference).
- **Memory Errors:**
  - Garbage values, Segfaults, Buffer overflow.

---

## 🖥️ Dynamic Memory Allocation
- **malloc:** reserves bytes from heap, returns pointer.
- **free:** releases memory back to system.
- **NULL pointers:** check after malloc to avoid errors.

---

## 🛠️ Tools and File I/O
- **Valgrind:** Detects memory leaks, invalid accesses.
- **File I/O:**
  - `FILE *` → file pointer
  - `fopen(), fclose(), fread(), fwrite(), fprintf()` 
- **Pointers in File I/O:** Essential for reading/writing bytes.

---

## ⚡ Conclusion: Power and Responsibility
- Pointers provide deep control over memory but can easily cause errors.
- Concepts applied in weekly problem set: manipulate bitmap image bytes (filters like grayscale, sepia, blur).
- Understanding memory is key for mastering low-level programming in C.

---

> ✅ **Pro Tip for GitHub:** Save this as `CS50x_2025_Lecture4_README.md`. Markdown renders beautifully on GitHub.
"""

# Save to file
readme_path = Path("CS50x_2025_Lecture4_README.md")
readme_path.write_text(readme_content, encoding="utf-8")
print(f"File saved to: {readme_path.resolve()}")
