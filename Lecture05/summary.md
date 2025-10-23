# 📚 CS50 Lecture 5: Data Structures

> **Course**: CS50 - Harvard University's Introduction to Computer Science  
> **Lecture**: Week 5 - Data Structures  
> **Focus**: Memory organization, dynamic data structures, and algorithm efficiency

---

## 🎯 Learning Objectives

By the end of this lecture, you will understand:
- Abstract Data Types vs. Data Structures
- Memory efficiency and trade-offs
- Dynamic memory allocation strategies
- Core data structures: Linked Lists, Trees, Hash Tables, and Tries
- Time and space complexity considerations

---

## 📑 Table of Contents

1. [Abstract Data Types](#-abstract-data-types)
   - [Queues (FIFO)](#queues-fifo)
   - [Stacks (LIFO)](#stacks-lifo)
2. [The Problem with Arrays](#-the-problem-with-arrays)
3. [Resizing Arrays](#-resizing-arrays)
4. [Linked Lists](#-linked-lists)
5. [Binary Search Trees](#-binary-search-trees)
6. [Hash Tables](#-hash-tables)
7. [Tries](#-tries)
8. [Performance Comparison](#-performance-comparison)

---

## 🔄 Abstract Data Types

**Abstract Data Types (ADTs)** are high-level descriptions of data structures that define operations and properties, independent of implementation details.

### Queues (FIFO)

> **FIFO**: First In, First Out

**Real-world examples**: 
- 🎬 Movie theater lines
- 🖨️ Printer job queues
- 🍔 Restaurant ordering systems

**Operations**:
- **Enqueue**: Add element to the back of the queue
- **Dequeue**: Remove element from the front of the queue

**Properties**:
- ✅ Fair and equitable (first come, first served)
- ✅ Preserves order of arrival

**C Implementation Structure**:
```c
typedef struct {
    person people[CAPACITY];
    int size;
} queue;
```

**Key Learning**: Queues ensure fairness by processing items in the exact order they arrive.

---

### Stacks (LIFO)

> **LIFO**: Last In, First Out

**Real-world examples**: 
- 📧 Email inbox (most recent on top)
- 🍽️ Stack of plates in a cafeteria
- 👕 Pile of laundry

**Operations**:
- **Push**: Add element to the top of the stack
- **Pop**: Remove element from the top of the stack

**Properties**:
- ⚠️ Not equitable (most recent items processed first)
- ⚠️ Older items may get "buried"

**C Implementation Structure**:
```c
typedef struct {
    person people[CAPACITY];
    int size;
} stack;
```

**Key Learning**: The only difference between queue and stack implementation is *where* you add/remove elements in the underlying array.

---

## ⚠️ The Problem with Arrays

Arrays are **contiguous blocks of memory** with a **fixed size** determined at creation.

### Visual Representation

```
[1][2][3] ← Our array
         [H][e][l][l][o][ ][w][o][r][l][d][\0] ← Another variable
         ↑ Can't insert [4] here!
```

### Key Issues:

1. **❌ Fixed Capacity**: Must decide size in advance
2. **❌ No Room to Grow**: Adjacent memory might be occupied
3. **❌ Wasteful Over-allocation**: Allocating extra space wastes memory
4. **❌ Expensive Resizing**: Copying entire array is O(n) operation

---

## 🔄 Resizing Arrays

### The Process

1. **Allocate** new, larger chunk of memory
2. **Copy** all elements from old to new array
3. **Add** new element(s)
4. **Free** old memory

### Code Example

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    // Initial allocation
    int *list = malloc(3 * sizeof(int));
    if (list == NULL) {
        return 1;
    }
    
    list[0] = 1;
    list[1] = 2;
    list[2] = 3;
    
    // --- Time passes, need more space ---
    
    // Allocate new array
    int *tmp = malloc(4 * sizeof(int));
    if (tmp == NULL) {
        free(list);  // Don't leak memory!
        return 1;
    }
    
    // Copy old values
    for (int i = 0; i < 3; i++) {
        tmp[i] = list[i];
    }
    
    // Add new value
    tmp[3] = 4;
    
    // Clean up
    free(list);
    list = tmp;
    
    // Use list...
    for (int i = 0; i < 4; i++) {
        printf("%i\n", list[i]);
    }
    
    free(list);
    return 0;
}
```

### 📊 Performance Analysis

- **Time Complexity**: O(n) - must copy n elements
- **Space Complexity**: 2n temporarily (old + new)
- **Problem**: Gets exponentially worse with repeated resizing

### Alternative: `realloc()`

```c
int *tmp = realloc(list, 4 * sizeof(int));
if (tmp == NULL) {
    free(list);
    return 1;
}
list = tmp;
list[3] = 4;
```

**Advantage**: If memory is available adjacent to original allocation, `realloc()` may not need to copy!

---

## 🔗 Linked Lists

A **linked list** uses pointers to connect non-contiguous chunks of memory, eliminating the need for resizing.

### Structure

```
[1]──→[2]──→[3]──→NULL
```

Each element (node) contains:
- **Data** (e.g., an integer)
- **Pointer** to the next node

### Node Definition

```c
typedef struct node {
    int number;
    struct node *next;
} node;
```

**Note**: The `struct node` must be used for the `next` pointer because C reads top-to-bottom.

### Pointer Syntax

```c
// Dereferencing and accessing member
(*n).number = 1;

// Arrow operator (shorthand)
n->number = 1;  // Equivalent to above
```

### Building a Linked List

#### 1️⃣ Initialize Empty List

```c
node *list = NULL;
```

#### 2️⃣ Create First Node

```c
node *n = malloc(sizeof(node));
if (n == NULL) {
    return 1;
}

n->number = 1;
n->next = NULL;

list = n;  // Point list to first node
```

#### 3️⃣ Prepend (Insert at Beginning)

```c
node *n = malloc(sizeof(node));
if (n == NULL) {
    return 1;
}

n->number = 2;
n->next = list;  // New node points to current head
list = n;        // Update head to new node
```

**Time Complexity**: O(1) - constant time!

#### 4️⃣ Append (Insert at End)

```c
node *n = malloc(sizeof(node));
if (n == NULL) {
    return 1;
}

n->number = 4;
n->next = NULL;

if (list == NULL) {
    list = n;  // Empty list
} else {
    // Traverse to end
    node *ptr = list;
    while (ptr->next != NULL) {
        ptr = ptr->next;
    }
    ptr->next = n;
}
```

**Time Complexity**: O(n) - must traverse entire list

#### 5️⃣ Insert in Sorted Order

```c
node *n = malloc(sizeof(node));
if (n == NULL) {
    return 1;
}

n->number = value;
n->next = NULL;

// Empty list or insert at beginning
if (list == NULL || value < list->number) {
    n->next = list;
    list = n;
} else {
    // Find insertion point
    node *ptr = list;
    while (ptr->next != NULL && ptr->next->number < value) {
        ptr = ptr->next;
    }
    n->next = ptr->next;
    ptr->next = n;
}
```

**Time Complexity**: O(n) - worst case traverse entire list

### ⚖️ Trade-offs

| Feature | Arrays | Linked Lists |
|---------|--------|-------------|
| **Memory** | Contiguous | Non-contiguous |
| **Size** | Fixed | Dynamic |
| **Insert at beginning** | O(n) | O(1) |
| **Search** | O(log n) with binary search | O(n) |
| **Random access** | O(1) | O(n) |
| **Memory overhead** | None | Pointers (extra space) |

**Key Insight**: Linked lists trade **space** (extra pointers) for **time** (no resizing needed).

---

## 🌳 Binary Search Trees

**Goal**: Combine the best of arrays (fast search) and linked lists (dynamic growth).

### Structure

```
       4
      / \
     2   6
    / \ / \
   1  3 5  7
```

### Properties

- Each node has **at most two children** (left and right)
- **Left subtree**: All values < parent
- **Right subtree**: All values > parent
- **Recursive**: Each subtree is itself a BST

### Node Definition

```c
typedef struct node {
    int number;
    struct node *left;
    struct node *right;
} node;
```

### Search Algorithm (Recursive)

```c
bool search(node *tree, int number) {
    if (tree == NULL) {
        return false;  // Not found
    } else if (number < tree->number) {
        return search(tree->left, number);
    } else if (number > tree->number) {
        return search(tree->right, number);
    } else {
        return true;  // Found!
    }
}
```

### 📊 Performance

- **Best/Average Case**: O(log n) - eliminates half the tree at each step
- **Worst Case**: O(n) - degenerates into a linked list if unbalanced

### ⚠️ The Balancing Problem

**Bad insertion order** (e.g., 1, 2, 3, 4, 5):
```
1
 \
  2
   \
    3
     \
      4
       \
        5
```
This is effectively a linked list! Search becomes O(n).

**Solution**: Self-balancing trees (AVL, Red-Black) - advanced topic.

---

## 🗂️ Hash Tables

**Goal**: Achieve O(1) constant-time operations.

### Concept

A **hash table** combines:
- **Array**: Fast indexed access
- **Linked Lists**: Handle collisions

### Structure

```
Index 0: [Alice] → [Anna] → NULL
Index 1: [Bob] → NULL
Index 2: [Charlie] → [Chris] → NULL
...
Index 25: [Zelda] → NULL
```

### Hash Function Example

```c
// Simple hash: first letter
unsigned int hash(const char *word) {
    if (isupper(word[0])) {
        return word[0] - 'A';  // 0-25
    } else if (islower(word[0])) {
        return word[0] - 'a';  // 0-25
    }
    return 0;
}
```

### Node Structure

```c
typedef struct node {
    char name[LENGTH + 1];
    char number[LENGTH + 1];
    struct node *next;
} node;

node *table[26];  // Array of linked list heads
```

### 📊 Performance

- **Best Case**: O(1) - no collisions, direct access
- **Worst Case**: O(n) - all items hash to same bucket
- **Average Case**: O(n/k) where k = number of buckets

### Trade-off

**More buckets** → Fewer collisions → Faster access → More memory

Example:
- 26 buckets (A-Z): Moderate performance
- 17,576 buckets (AAA-ZZZ): Better performance, 676× more memory
- Infinite buckets: O(1) guaranteed, infinite memory ❌

**Key Insight**: Hash tables are widely used because they offer *practical* O(1) performance with reasonable memory usage.

---

## 🌲 Tries

**Trie** (pronounced "try"): A tree of arrays, optimized for string keys.

### Structure

```
Root Array [A][B][C]...[Z]
            ↓
            [A][B][C]...[Z]  (for words starting with 'A')
                 ↓
                 [A][B][C]...[Z]  (for words starting with 'AB')
```

### Example: Storing "Toad", "Toadette", "Tom"

```
        [T]
         ↓
        [O]
         ↓
        [A][M]
         ↓   ↓
        [D] (word)
         ↓
        [E]
         ↓
        [T]
         ↓
        [T]
         ↓
        [E] (word)
```

### Node Structure

```c
typedef struct node {
    char *number;           // Or bool is_word
    struct node *children[26];  // One for each letter
} node;
```

### 📊 Performance

- **Search/Insert/Delete**: O(k) where k = length of key
- Since k is bounded (e.g., max word length ~45 letters), this is effectively **O(1)**

### ⚖️ Trade-off

**Pros**:
- ✅ Incredibly fast lookups
- ✅ No collisions
- ✅ Effectively constant time

**Cons**:
- ❌ **Enormous memory usage**
- ❌ Many null pointers (wasted space)

**Example**: Storing just "Toad" requires:
- 1 root array (26 pointers)
- 1 'T' array (26 pointers)
- 1 'O' array (26 pointers)
- 1 'A' array (26 pointers)
- 1 'D' array (26 pointers)
- **Total**: 130 pointers, most are NULL!

---

## 📊 Performance Comparison

| Data Structure | Search | Insert (unsorted) | Insert (sorted) | Delete | Space |
|----------------|--------|-------------------|-----------------|--------|-------|
| **Array** | O(log n)* | O(1)** | O(n) | O(n) | Minimal |
| **Linked List** | O(n) | O(1) at head | O(n) | O(n) | Pointers |
| **Binary Search Tree** | O(log n)*** | O(log n)*** | O(log n)*** | O(log n)*** | Pointers |
| **Hash Table** | O(1)**** | O(1)**** | N/A | O(1)**** | Moderate |
| **Trie** | O(k) | O(k) | O(k) | O(k) | Very High |

*With binary search  
**Insert at end  
***If balanced  
****Average case; O(n) worst case  
k = length of key

---

## 💡 Key Takeaways

### 1️⃣ **Trade-offs are Everywhere**

There is no "best" data structure - only trade-offs:
- **Time vs. Space**: Tries are fast but memory-hungry
- **Simplicity vs. Performance**: Arrays are simple but inflexible
- **Worst-case vs. Average-case**: Hash tables are usually fast, but not always

### 2️⃣ **Memory is Not Free**

Every data structure has hidden costs:
- Linked lists: Extra pointers
- Hash tables: Empty buckets
- Tries: Massive arrays of mostly NULL pointers

### 3️⃣ **Implementation Matters**

The same ADT can be implemented differently:
- Queues and stacks can both use arrays
- Dictionaries can be arrays, linked lists, hash tables, or tries
- Choice depends on use case

### 4️⃣ **Real-World Example**

**Sweetgreen Salad Pickup**: Orders organized by first letter of name
- This is a **hash table**!
- Fast O(1) average lookup
- Simple hash function (first letter)
- Collisions handled by scanning shelf

---

## 🔧 Practical Recommendations

### When to Use Each Data Structure:

| Use Case | Recommended Structure |
|----------|----------------------|
| Fixed-size list | **Array** |
| Frequent insertions at beginning | **Linked List** |
| Need sorted data + fast search | **Binary Search Tree** |
| Dictionary/map with fast lookup | **Hash Table** |
| Autocomplete, spell-check | **Trie** |
| Recent items priority | **Stack** |
| Fair ordering (tickets, tasks) | **Queue** |

---

## 🎓 Looking Ahead

### Week 6: Python

Higher-level languages like Python **abstract away** these implementation details:
- Python's `list` handles resizing automatically
- Python's `dict` is a hash table under the hood
- You focus on *what* to do, not *how* to do it

**But understanding C data structures helps you**:
- Choose the right Python data structure
- Understand performance implications
- Debug memory issues
- Optimize bottlenecks

---

## 📚 Additional Resources

- **CS50 Manual Pages**: `man malloc`, `man free`
- **Visualizations**: [visualgo.net](https://visualgo.net)
- **Practice**: CS50 Problem Set 5 (Speller)

---

## 🎬 Lecture Credits

**Instructor**: David J. Malan  
**University**: Harvard University  
**Course**: CS50x 2024  
**Special Thanks**: Shannon Duvall (Elon University) for "Jack Learns the Facts" video

---

## 📝 Notes

This summary is based on CS50 Lecture 5 (2024). The lecture covers fundamental concepts that remain consistent across programming languages and are essential for technical interviews and real-world software development.

**Remember**: The best programmers don't just know *how* to implement these structures - they know *when* to use each one.

---

*Made with 💙 for CS50 students learning data structures*
