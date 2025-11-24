# 🧮 Assignment 18: Bubble Sort Using Doubly Linked List

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
To implement **Bubble Sort** on a **Doubly Linked List** in C++, sort a list of integers, and display the list before and after sorting.

---

## 📝 Problem Statement

Write a C++ program to:

1. Create a **doubly linked list** of integers.  
2. Insert elements at the **end** of the list.  
3. Apply **Bubble Sort** on the doubly linked list to sort elements in **ascending order**.  
4. Display:
   - The **original list** (before sorting)  
   - The **sorted list** (after applying Bubble Sort)

---

## 📘 Theory

### 🔹 Doubly Linked List

A **doubly linked list** is a linear data structure where each node contains:

- `data_ssd` → stores an integer value  
- `prev_ssd` → pointer to the **previous node**  
- `next_ssd` → pointer to the **next node**

It allows traversal in both directions (forward and backward).

### 🔹 Bubble Sort

**Bubble Sort** is a simple comparison-based sorting algorithm:

- Repeatedly traverses the list.
- Compares **adjacent elements**.
- Swaps them if they are in the **wrong order**.
- After each pass, the **largest element** moves to its correct position at the end.

In this implementation:

- We perform Bubble Sort **directly on the doubly linked list**.
- Instead of changing links, we **swap the data values** of nodes.

---

## 🧠 Algorithm

### 1️⃣ Creation and Insertion

1. Define a node structure with:
   - `int data_ssd;`
   - `Node_ssd* prev_ssd;`
   - `Node_ssd* next_ssd;`
2. Use `insertEnd_ssd` to:
   - Create a new node using `createNode_ssd`.
   - If list is empty → new node becomes `head_ssd`.
   - Else → traverse to the last node and attach new node at the end.

---

### 2️⃣ Bubble Sort on Doubly Linked List

`bubbleSort_ssd(head_ssd)`:

1. If list is empty → return.  
2. Use:
   - `bool swapped` to check if any swap occurred in a pass.
   - `Node_ssd* lptr_ssd = nullptr;` to mark the **last sorted node**.
3. Repeat:
   - Set `swapped = false`.  
   - Start from `ptr1_ssd = head_ssd`.  
   - While `ptr1_ssd->next_ssd != lptr_ssd`:
     - If `ptr1_ssd->data_ssd > ptr1_ssd->next_ssd->data_ssd`:
       - Swap their `data_ssd` values.
       - Set `swapped = true`.
     - Move `ptr1_ssd` one step forward.
   - Set `lptr_ssd = ptr1_ssd` (last node of this pass is now in correct position).  
4. Continue passes until `swapped` becomes `false`.

---

## 💻 C++ Program

```cpp
#include <iostream>
using namespace std;

// Node structure for doubly linked list
struct Node_ssd {
    int data_ssd;
    Node_ssd* prev_ssd;
    Node_ssd* next_ssd;
};

// Create a new node
Node_ssd* createNode_ssd(int data_ssd) {
    Node_ssd* newNode_ssd = new Node_ssd;
    newNode_ssd->data_ssd = data_ssd;
    newNode_ssd->prev_ssd = nullptr;
    newNode_ssd->next_ssd = nullptr;
    return newNode_ssd;
}

// Insert node at the end
Node_ssd* insertEnd_ssd(Node_ssd* head_ssd, int data_ssd) {
    Node_ssd* newNode_ssd = createNode_ssd(data_ssd);
    if (!head_ssd)
        return newNode_ssd;

    Node_ssd* temp_ssd = head_ssd;
    while (temp_ssd->next_ssd)
        temp_ssd = temp_ssd->next_ssd;

    temp_ssd->next_ssd = newNode_ssd;
    newNode_ssd->prev_ssd = temp_ssd;
    return head_ssd;
}

// Print the list
void printList_ssd(Node_ssd* head_ssd) {
    Node_ssd* temp_ssd = head_ssd;
    while (temp_ssd) {
        cout << temp_ssd->data_ssd << " ";
        temp_ssd = temp_ssd->next_ssd;
    }
    cout << endl;
}

// Bubble sort on doubly linked list
void bubbleSort_ssd(Node_ssd* head_ssd) {
    if (!head_ssd) return;

    bool swapped;
    Node_ssd* ptr1_ssd;
    Node_ssd* lptr_ssd = nullptr; // Last sorted node

    do {
        swapped = false;
        ptr1_ssd = head_ssd;

        while (ptr1_ssd->next_ssd != lptr_ssd) {
            if (ptr1_ssd->data_ssd > ptr1_ssd->next_ssd->data_ssd) {
                // Swap data
                int temp_ssd = ptr1_ssd->data_ssd;
                ptr1_ssd->data_ssd = ptr1_ssd->next_ssd->data_ssd;
                ptr1_ssd->next_ssd->data_ssd = temp_ssd;
                swapped = true;
            }
            ptr1_ssd = ptr1_ssd->next_ssd;
        }
        lptr_ssd = ptr1_ssd; // Last node is in correct position
    } while (swapped);
}

// Main function
int main() {
    Node_ssd* head_ssd = nullptr;
    int n_ssd, value_ssd;

    cout << "Enter number of elements: ";
    cin >> n_ssd;

    cout << "Enter elements:\n";
    for (int i = 0; i < n_ssd; i++) {
        cin >> value_ssd;
        head_ssd = insertEnd_ssd(head_ssd, value_ssd);
    }

    cout << "Original list: ";
    printList_ssd(head_ssd);

    bubbleSort_ssd(head_ssd);

    cout << "Sorted list: ";
    printList_ssd(head_ssd);

    return 0;
}
```

---

## 🧪 Example Execution

```text
Enter number of elements: 10
Enter elements:
10 99 18 2 54 33 9 20 72 11

Original list: 10 99 18 2 54 33 9 20 72 11 
Sorted list: 2 9 10 11 18 20 33 54 72 99 
```

Program successfully sorts the list in ascending order using **Bubble Sort** on a **doubly linked list**.

---

## 🧠 Memory Visualization

For input:

```text
10 99 18 2 54 33 9 20 72 11
```

### Initial Doubly Linked List

```text
[10] <-> [99] <-> [18] <-> [2] <-> [54] <-> [33] <-> [9] <-> [20] <-> [72] <-> [11] -> NULL
```

Each node:

| Field        | Description             |
|--------------|-------------------------|
| `data_ssd`   | Integer value           |
| `prev_ssd`   | Pointer to previous node|
| `next_ssd`   | Pointer to next node    |

After Bubble Sort, the list becomes:

```text
[2] <-> [9] <-> [10] <-> [11] <-> [18] <-> [20] <-> [33] <-> [54] <-> [72] <-> [99] -> NULL
```

---

## ✅ Conclusion

This assignment demonstrates:

- Implementation of a **doubly linked list** with dynamic insertion at the end.
- Use of **Bubble Sort** on a linked structure by **swapping data values**.
- Traversal and display of list content **before and after sorting**.

It strengthens understanding of **linked list operations**, **sorting algorithms**, and pointer-based data structures in C++.

