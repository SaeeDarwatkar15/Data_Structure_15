# 🧩 Assignment 15: Binary Number Operations Using Doubly Linked List

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment stores a **binary number** using a **doubly linked list** and performs:
- **1’s complement**  
- **2’s complement**  
- **Addition of two binary numbers** (both stored as doubly linked lists)

---

## 📝 Problem Statement

Write a **C++ program** to:

1. Store a **binary number** using a **doubly linked list** (each node = 1 bit).  
2. Implement functions to:
   - **Calculate and display** the **1’s complement** and **2’s complement** of the stored binary number.
   - **Add two binary numbers**, each represented using a doubly linked list, and **display the result**.

Binary numbers are entered as a sequence of bits from **MSB to LSB**.

---

## 📘 Theory

### 🔹 Doubly Linked List Representation of Binary Numbers

A binary number like `10111` is stored as:

```text
1 <-> 0 <-> 1 <-> 1 <-> 1
^
Head (MSB)
```

Each node (`struct Node`) stores:
- `bit` → either `0` or `1`
- `prev` → pointer to previous node
- `next` → pointer to next node

Advantages:
- Easy traversal from **MSB to LSB** (forward) or **LSB to MSB** (backward).
- Perfect for operations like **addition** and **2’s complement**, which work from the **rightmost bit (LSB)**.

---

### 🔹 1’s Complement

**Definition:**  
1’s complement of a binary number is obtained by **flipping every bit**:

- `0` → `1`
- `1` → `0`

**Example:**  
`10111` → `01000`

In the linked list:
- Traverse each node.
- Replace `bit` with `1 - bit` (or `bit ? 0 : 1`).

---

### 🔹 2’s Complement

**Definition:**  
2’s complement of a binary number is:

> 2’s complement = 1’s complement + 1

Steps:
1. Compute **1’s complement** of the number.  
2. Add **1** starting from the **LSB**, managing carry:

   - If bit = 0 + 1 → 1, carry = 0  
   - If bit = 1 + 1 → 0, carry = 1  

In the linked list:
- First compute a **separate 1’s complement list**.
- Move to the **last node** (LSB).
- Perform addition with carry **backwards** using `prev`.

---

### 🔹 Binary Addition Using Doubly Linked Lists

We want to add two binary numbers:

- Start from **LSB** of both lists.
- For each bit:

  ```text
  sum = bitA + bitB + carry
  resultBit = sum % 2
  carry = sum / 2
  ```

- Insert result bits into a **new list**.
- Finally, **reverse** the result list to restore MSB→LSB order.

Example:  
`10111` (23)  
`0101`  (5)  

Result: `11100` (28)

---

## 🧠 Algorithm (Step-by-Step)

### 1️⃣ Store Binary Number

1. Accept `n` (number of bits).  
2. Repeat `n` times:
   - Read bit (`0` or `1`).
   - Insert at the **end** of doubly linked list using `insertEnd_ssd`.

---

### 2️⃣ 1’s Complement

1. Create a new empty list `result`.  
2. Traverse original list from head:
   - For each node:
     - If `bit == 0`, insert `1`.
     - If `bit == 1`, insert `0`.
3. Return head of new list.

---

### 3️⃣ 2’s Complement

1. Generate **1’s complement list** `ones`.  
2. Move to **last node** of `ones`.  
3. Initialize `carry = 1`, create empty `result`.  
4. While node exists:
   - `sum = bit + carry`
   - `resultBit = sum % 2`, `carry = sum / 2`
   - Insert resultBit at end of `result`.
   - Move to `prev`.  
5. If `carry == 1`, insert one more node with bit `1`.  
6. Reverse `result` (since it was built from LSB to MSB).  
7. Return reversed list head.

---

### 4️⃣ Binary Addition (Two Lists)

1. Move both pointers (`tempA`, `tempB`) to **LSB** (last node).  
2. Initialize `carry = 0`, `result = NULL`.  
3. While `tempA` or `tempB` exists:
   - `bitA = tempA ? tempA->bit : 0`
   - `bitB = tempB ? tempB->bit : 0`
   - `sum = bitA + bitB + carry`
   - Insert `sum % 2` into result (end).
   - `carry = sum / 2`
   - Move `tempA = tempA->prev`, `tempB = tempB->prev`.  
4. If `carry == 1`, add extra node.  
5. Reverse result list to get MSB→LSB order.  

---

## 💻 C++ Program

```cpp
#include <iostream>
using namespace std;

// Doubly linked list node
struct Node {
    int bit;
    Node* prev;
    Node* next;
};

// Create a new node
Node* createNode_ssd(int bit) {
    Node* newNode = new Node();
    newNode->bit = bit;
    newNode->prev = newNode->next = nullptr;
    return newNode;
}

// Insert node at end
Node* insertEnd_ssd(Node* head, int bit) {
    Node* newNode = createNode_ssd(bit);
    if (!head) return newNode;

    Node* temp = head;
    while (temp->next) temp = temp->next;
    temp->next = newNode;
    newNode->prev = temp;
    return head;
}

// Print linked list
void printList_ssd(Node* head) {
    Node* temp = head;
    while (temp) {
        cout << temp->bit;
        temp = temp->next;
    }
    cout << endl;
}

// 1's complement
Node* onesComplement_ssd(Node* head) {
    Node* temp = head;
    Node* result = nullptr;
    while (temp) {
        result = insertEnd_ssd(result, temp->bit ? 0 : 1);
        temp = temp->next;
    }
    return result;
}

// 2's complement
Node* twosComplement_ssd(Node* head) {
    Node* ones = onesComplement_ssd(head);

    // Move to LSB
    Node* temp = ones;
    while (temp->next) temp = temp->next;

    Node* result = nullptr;
    int carry = 1;

    while (temp) {
        int sum = temp->bit + carry;
        result = insertEnd_ssd(result, sum % 2);
        carry = sum / 2;
        temp = temp->prev;
    }

    if (carry) result = insertEnd_ssd(result, carry);

    // Reverse the result
    Node* prev = nullptr;
    Node* curr = result;
    while (curr) {
        Node* next = curr->next;
        curr->next = prev;
        curr->prev = next;
        prev = curr;
        curr = next;
    }

    return prev;
}

// Add two binary numbers
Node* addBinary_ssd(Node* A, Node* B) {
    // Move to LSB
    Node* tempA = A;
    Node* tempB = B;
    while (tempA && tempA->next) tempA = tempA->next;
    while (tempB && tempB->next) tempB = tempB->next;

    Node* result = nullptr;
    int carry = 0;

    while (tempA || tempB) {
        int bitA = tempA ? tempA->bit : 0;
        int bitB = tempB ? tempB->bit : 0;

        int sum = bitA + bitB + carry;
        result = insertEnd_ssd(result, sum % 2);
        carry = sum / 2;

        if (tempA) tempA = tempA->prev;
        if (tempB) tempB = tempB->prev;
    }

    if (carry) result = insertEnd_ssd(result, carry);

    // Reverse result
    Node* prev = nullptr;
    Node* curr = result;
    while (curr) {
        Node* next = curr->next;
        curr->next = prev;
        curr->prev = next;
        prev = curr;
        curr = next;
    }

    return prev;
}

// Main function
int main() {
    Node* bin1 = nullptr;
    Node* bin2 = nullptr;
    int n, bit;

    cout << "Enter number of bits in first binary number: ";
    cin >> n;
    cout << "Enter bits (MSB to LSB): ";
    for (int i = 0; i < n; i++) {
        cin >> bit;
        bin1 = insertEnd_ssd(bin1, bit);
    }

    cout << "Enter number of bits in second binary number: ";
    cin >> n;
    cout << "Enter bits (MSB to LSB): ";
    for (int i = 0; i < n; i++) {
        cin >> bit;
        bin2 = insertEnd_ssd(bin2, bit);
    }

    cout << "\nFirst Binary Number: ";
    printList_ssd(bin1);

    Node* ones = onesComplement_ssd(bin1);
    cout << "1's Complement: ";
    printList_ssd(ones);

    Node* twos = twosComplement_ssd(bin1);
    cout << "2's Complement: ";
    printList_ssd(twos);

    cout << "\nSecond Binary Number: ";
    printList_ssd(bin2);

    Node* sum = addBinary_ssd(bin1, bin2);
    cout << "Addition of Two Binary Numbers: ";
    printList_ssd(sum);

    return 0;
}
```

---

## 🧪 Example Execution

```text
Enter number of bits in first binary number: 5
Enter bits (MSB to LSB): 1 0 1 1 1
Enter number of bits in second binary number: 4
Enter bits (MSB to LSB): 0 1 0 1

First Binary Number: 10111
1's Complement: 01000
2's Complement: 01001

Second Binary Number: 0101
Addition of Two Binary Numbers: 11100
```

`10111` (23) + `0101` (5) = `11100` (28) ✅

---

## 🧠 Memory Visualization

### Node Structure

Each node in the doubly linked list:

| Field | Description               |
|-------|---------------------------|
| bit   | Single binary bit (0/1)   |
| prev  | Pointer to previous node  |
| next  | Pointer to next node      |

### Example: First Binary Number = `10111`

Stored as:

```text
[1] <-> [0] <-> [1] <-> [1] <-> [1]
 ^                               ^
 head (MSB)                      tail (LSB)
```

### During 2’s Complement:

1. **1’s Complement**: `01000`

```text
[0] <-> [1] <-> [0] <-> [0] <-> [0]
```

2. Start from **LSB**, add 1:

- 0 + 1 → 1, carry = 0  
- Remaining bits copied as they are.

Result (before reverse, stored backward as we build from LSB):  
`10010` (reversed form)

After reversing, final **2’s complement**:  
`01001`

---

## ✅ Conclusion

This assignment demonstrates:

- How to represent **binary numbers using doubly linked lists**.
- How to compute **1’s complement** and **2’s complement** using bitwise logic and list traversal.
- How to perform **binary addition** using:
  - Backward traversal from **LSB using `prev` pointers**.
  - Managing **carry** and building a new result list.
- Reversal of lists to maintain **MSB to LSB** order for correct display.

It gives a strong understanding of combining **linked lists** with **binary arithmetic** operations in C++.

