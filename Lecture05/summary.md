# CS50x 2025 – Lecture 5: Data Structures

## Overview

Lecture 5 marks the **final week of C programming** in CS50x and focuses on one of the most crucial topics in computer science: **data structures**.  
Data structures are **ways of organizing data in memory** to make it easier and faster to work with.  
This lecture ties together the key ideas from previous weeks — **memory**, **pointers**, and **structs** — to show how complex data can be managed efficiently.

---

## 1. What Are Data Structures?

A **data structure** is a way to store and organize data so that it can be accessed and modified efficiently.  
Think of them as **containers** for related pieces of information.

### Common Data Structures:
- **Arrays** – fixed-size collections of elements.
- **Linked Lists** – dynamic collections connected via pointers.
- **Stacks** – LIFO (Last In, First Out) systems.
- **Queues** – FIFO (First In, First Out) systems.
- **Trees** – hierarchical structures.
- **Hash Tables** – store data for fast lookup using keys.

Each structure offers **different trade-offs** between memory usage, speed, and flexibility.

---

## 2. Review of Memory Concepts

Before diving into data structures, CS50 revisits **memory management**, since all data structures rely on how memory is used.

### Key Concepts:
- **Pointers**: store memory addresses of other variables.
- **malloc()**: dynamically allocates memory on the heap.
- **free()**: releases allocated memory to prevent leaks.
- **Segmentation Faults**: occur when accessing memory incorrectly.

Understanding these is critical because **dynamic data structures** (like linked lists or trees) require careful control over memory allocation.

---

## 3. Linked Lists

### The Problem with Arrays:
Arrays have a **fixed size**.  
If you want to store more elements than expected, you must create a **new, larger array** and copy the old elements — inefficient.

### The Linked List Solution:
A **linked list** stores data in a sequence of **nodes** connected by **pointers**.

Each **node** contains:
```c
typedef struct node {
    int number;
    struct node *next;
} node;
```

- `number` – stores the data.
- `next` – pointer to the next node in the list.

### Visualization:
```
[10 | *] → [20 | *] → [30 | NULL]
```

### Benefits:
- **Dynamic size**: can grow or shrink easily.
- **Efficient insertion**: no need to shift elements.

### Drawbacks:
- **No random access**: must traverse sequentially.
- **Extra memory**: each node stores a pointer.

---

## 4. Building a Linked List (Example)

### 1. Create the First Node:
```c
node *list = NULL;
node *n = malloc(sizeof(node));
if (n != NULL) {
    n->number = 1;
    n->next = NULL;
    list = n;
}
```

### 2. Add Another Node:
```c
n = malloc(sizeof(node));
if (n != NULL) {
    n->number = 2;
    n->next = NULL;
    list->next = n;
}
```

Now `list` points to a sequence of nodes:
```
[1] → [2] → NULL
```

### 3. Traversing the List:
```c
for (node *tmp = list; tmp != NULL; tmp = tmp->next)
    printf("%i\n", tmp->number);
```

### 4. Freeing the List:
```c
while (list != NULL) {
    node *tmp = list->next;
    free(list);
    list = tmp;
}
```
Freeing prevents **memory leaks**.

---

## 5. Searching in a Linked List

To find a specific value:
```c
bool search(node *list, int target) {
    for (node *tmp = list; tmp != NULL; tmp = tmp->next) {
        if (tmp->number == target) return true;
    }
    return false;
}
```

### Time Complexity:
- **O(n)**: must check each element one by one.

---

## 6. Abstraction with Data Structures

Instead of manually creating structures, we can **abstract** operations:
- **Insert()**
- **Delete()**
- **Search()**
- **Print()**

This improves readability and reuse.

---

## 7. Hash Tables

A **hash table** combines **arrays** and **linked lists** for faster lookups.

### Structure:
- Each array element (called a *bucket*) stores a **linked list**.
- A **hash function** determines where to store each value.

Example:
```c
int hash(string word) {
    return toupper(word[0]) - 'A';
}
```

If “Alice” hashes to bucket 0 and “Bob” hashes to bucket 1, then:
```
Bucket[0]: Alice → NULL
Bucket[1]: Bob → NULL
```

### Advantages:
- Faster lookups: ideally **O(1)**.
- Good for large datasets (like dictionaries).

### Disadvantages:
- **Collisions** can occur when multiple items hash to the same bucket.
- Requires careful hash function design.

---

## 8. Tries (Prefix Trees)

A **Trie** is a **tree-like structure** used for **storing words or prefixes**, like in a dictionary.

Each node represents a **character**, and paths represent **words**.

### Example:
For the words `cat`, `car`, and `dog`:

```
(root)
 ├── c
 │    └── a
 │         ├── t
 │         └── r
 └── d
      └── o
           └── g
```

### Benefits:
- Efficient prefix searches (e.g., autocomplete).
- Commonly used in spell-checkers and search engines.

---

## 9. Stacks and Queues

### Stack
- **LIFO** (Last In, First Out)
- Think of a stack of plates.
- Operations:
  - `push()` – add an item.
  - `pop()` – remove the last added item.

### Queue
- **FIFO** (First In, First Out)
- Think of a waiting line.
- Operations:
  - `enqueue()` – add an item.
  - `dequeue()` – remove the oldest item.

Both can be implemented using **arrays or linked lists**.

---

## 10. Trees

A **tree** is a hierarchical structure made of **nodes** connected by edges.

### Basic Terms:
- **Root**: top node.
- **Parent / Child**: relationships between nodes.
- **Leaf**: node with no children.

### Binary Search Tree (BST)
Each node has:
- A **value**.
- A **left child** (values smaller).
- A **right child** (values larger).

This allows efficient searching:
- **O(log n)** on average.

---

## 11. Why Data Structures Matter

Choosing the right data structure determines:
- How fast your program runs.
- How efficiently it uses memory.
- How easy it is to modify and scale.

### Summary of Trade-Offs

| Data Structure | Access | Search | Insert | Delete | Memory |
|----------------|--------|--------|--------|--------|---------|
| Array | O(1) | O(n) | O(n) | O(n) | Low |
| Linked List | O(n) | O(n) | O(1) | O(1) | Medium |
| Hash Table | O(1) | O(1) | O(1) | O(1) | High |
| Tree (BST) | O(log n) | O(log n) | O(log n) | O(log n) | Medium |

---

## 12. Takeaways

- **Memory understanding** (malloc, free, pointers) is essential.
- **Linked lists** introduce dynamic data management.
- **Hash tables and tries** make searching faster.
- **Stacks, queues, and trees** model real-world problems.
- **Efficiency and abstraction** are key to scalable programming.

---

## 13. CS50 Problem Set Connection

This lecture ties into **Problem Set 5**, which focuses on:
- Implementing a **dictionary** using a **hash table**.
- Managing memory carefully.
- Measuring **lookup time** efficiency.

You’ll apply:
- `struct` for data representation.
- `malloc` and `free` for dynamic memory.
- Linked lists and hashing for storage.

---

## 14. Final Thoughts

Data structures are the **backbone of computer science**.  
They turn raw memory into **organized, searchable, and efficient systems**.

From **arrays** to **hash tables**, these tools are everywhere — in databases, search engines, operating systems, and your favorite apps.  
Mastering them is one of the most important steps in becoming a strong programmer.
