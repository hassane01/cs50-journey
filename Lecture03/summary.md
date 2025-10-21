# 🎓 CS50x 2025 – Lecture 3: Algorithms 🚀

This briefing document summarizes the key concepts and themes from **CS50x 2025 Lecture 3**.  
The lecture pivots away from introducing new C syntax to focus almost entirely on **the design, analysis, and implementation of algorithms**. It revisits the core idea that computer science is about **problem-solving efficiently**, showing how algorithmic design affects performance dramatically at scale.

---

## 🧭 I. Introduction: The Importance of Good Design

The lecture revisits the **phone book problem** from Week 0 to re-emphasize that correctness alone is not enough — **efficiency matters**.

### Key Ideas
- **Correctness vs. Design:** A program can be correct but still inefficient. Good design makes solutions *fast* and *scalable*.
- **Phone Book Algorithms:**
  | Algorithm | Method | Complexity | Notes |
  |------------|---------|-------------|-------|
  | Linear Search (1 page) | Check one by one | O(n) | Correct but slow |
  | Linear Search (2 pages) | Check two at a time | O(n/2) ≈ O(n) | Slightly faster, same scale |
  | Binary Search | Divide and conquer | O(log n) | Huge performance gain |

> “This week is really going to be about implementing algorithms, not only correctly but also efficiently.”

---

## ⚙️ II. Analyzing Algorithms: Asymptotic Notation

### 📈 Big O (O)
Describes the **worst-case** running time — the upper bound.  
Focuses on how performance scales as *n* grows.

- Linear Search → **O(n)**
- Binary Search → **O(log n)**

### 📉 Big Ω (Omega)
Describes the **best-case** running time — the lower bound.

- Linear Search → Ω(1) (if item is first)
- Binary Search → Ω(1) (if item is in the middle)

### ⚖️ Big Θ (Theta)
Used when the best and worst cases grow at the same rate.

---

## 🔢 III. The Problem of Sorting

Efficient searching like binary search requires **sorted data**, introducing the **sorting problem**.

- **Need for Sorting:** Enables fast searching. Common in databases, contact lists, etc.
- **Trade-off:** Sorting takes time upfront but saves time when searching multiple times.

> “If you expect to search often, it’s worth keeping the data sorted.”

---

## 🧮 IV. Two Simple Sorting Algorithms

### 🟡 Selection Sort
**Idea:** Find the smallest element in the unsorted list and move it to the front.

```c
for (int i = 0; i < n - 1; i++) {
    int min = i;
    for (int j = i + 1; j < n; j++) {
        if (array[j] < array[min])
            min = j;
    }
    swap(array[i], array[min]);
}
```
- **Runtime:** Θ(n²)
- **Simple**, but inefficient for large data sets.

---

### 🔵 Bubble Sort
**Idea:** Repeatedly compare and swap adjacent elements if they’re out of order.  
Each pass “bubbles” the largest element to the end.

```c
for (int i = 0; i < n - 1; i++) {
    bool swapped = false;
    for (int j = 0; j < n - i - 1; j++) {
        if (array[j] > array[j + 1]) {
            swap(array[j], array[j + 1]);
            swapped = true;
        }
    }
    if (!swapped) break;
}
```
- **Worst-case:** Θ(n²)
- **Best-case:** Ω(n) (if no swaps needed)

---

## 🧱 V. Structs and Custom Data Types

To handle complex data (like names + numbers), C provides **structs**.

### 🧩 Problem
Parallel arrays (one for names, one for numbers) are error-prone.

### ✅ Solution: Structs
```c
typedef struct {
    string name;
    string number;
} person;
```
- **typedef** creates a custom data type `person`.
- Access fields with dot notation: `people[0].name`.

---

## 🔁 VI. Recursion: A New Way of Thinking

### 💡 Definition
A function is **recursive** if it **calls itself**.

### 🧠 Core Components
- **Base Case:** Stops the recursion.
- **Recursive Step:** Solves a smaller version of the problem.

**Example: Drawing a pyramid**
> To draw a pyramid of height `n`, first draw one of height `n - 1`, then add the bottom row.

---

## 🧩 VII. A Better Sorting Algorithm: Merge Sort

A powerful, **recursive** and much faster sorting method.

### 📜 Algorithm Steps
1. **Base Case:** If ≤ 1 element → already sorted.
2. **Recursive Step:** Sort the left half, then the right half.
3. **Merge Step:** Combine the two sorted halves.

### 🧮 Merge Step
Compare the first elements of each sub-list, take the smaller, and repeat — O(n).

### ⚙️ Complexity
- **Time:** O(n log n)
- **Space:** Uses extra memory for merging.

### ⏳ Trade-off
Merge Sort is **much faster** than Θ(n²) sorts but uses more space.

---

## 🎬 VIII. Conclusion: The Power of Algorithmic Design

The lecture ends with a **visual comparison** of sorting speeds — showing just how much faster Merge Sort is compared to Selection and Bubble Sort.

> A well-designed algorithm isn’t just theory — it’s the difference between a program that finishes in seconds and one that would take years.

---

📘 **Summary Table**

| Algorithm | Best Case | Worst Case | Avg. Case | Type |
|------------|------------|-------------|------------|------|
| Linear Search | Ω(1) | O(n) | Θ(n) | Search |
| Binary Search | Ω(1) | O(log n) | Θ(log n) | Search |
| Selection Sort | Ω(n²) | O(n²) | Θ(n²) | Sort |
| Bubble Sort | Ω(n) | O(n²) | Θ(n²) | Sort |
| Merge Sort | Ω(n log n) | O(n log n) | Θ(n log n) | Sort |

---

✨ **Key Takeaway:**  
Algorithmic design defines the *efficiency* of your solution. The right algorithm can turn an impossible problem into a trivial one.
