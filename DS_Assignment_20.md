# 🧮 Assignment 20: Front–Back Split of a Singly Linked List

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
Write a C++ program to **split a singly linked list** into **two sublists** — a **front half** and a **back half** — such that:  
- If the number of nodes is **odd**, the **extra node** goes into the **front list**.  
- Example: `{2, 3, 5, 7, 11} → front = {2, 3, 5}, back = {7, 11}`.

---

## 📝 Problem Statement

Given a singly linked list with `n` elements, implement a function:

```cpp
void FrontBackSplit_ssd(node_ssd* source, node_ssd** frontRef, node_ssd** backRef);
```

This function should:

1. **Split** the original list `source` into two lists:  
   - `frontRef` → points to the first half  
   - `backRef` → points to the second half  

2. Rules:  
   - If `n < 2`, the **back list is empty**, and the **front list is the original list**.  
   - If `n` is **odd**, the **front list** has one more node than the back list.  
   - Function must work correctly for boundary cases:  
     - `n = 0`, `n = 1`, `n = 2`, `n = 3`, `n = 4`, etc.

3. The program should:  
   - Take input from the user.  
   - Build the linked list.  
   - Call `FrontBackSplit_ssd`.  
   - Display:  
     - Original list  
     - Front list  
     - Back list  

---

## 📘 Theory

### 🔹 Singly Linked List

Each node contains:

- `data_ssd` → integer value  
- `next_ssd` → pointer to the next node  

The last node has `next_ssd = nullptr`.

---

### 🔹 Front–Back Split Using Fast and Slow Pointers

To split the list into **front half** and **back half**:

- Use two pointers:
  - `slow` moves **one node at a time**.
  - `fast` moves **two nodes at a time**.
- When `fast` reaches the end:
  - `slow` will be at the **end of the front half**.
- Then:
  - `frontRef = source;`
  - `backRef = slow->next_ssd;`
  - Set `slow->next_ssd = nullptr;` to **terminate the front list**.

This technique correctly handles both even and odd-length lists.

---

### 🔹 Special Cases

1. **Empty list (`n = 0`)**  
   - `source == nullptr`  
   - `frontRef = source`  
   - `backRef = nullptr`

2. **Single node (`n = 1`)**  
   - Only one node in front list.  
   - Back list remains empty.

---

## 🧠 Algorithm (FrontBackSplit)

1. **Input**: head pointer `source`.  
2. If `source == nullptr` or `source->next_ssd == nullptr`:
   - `frontRef = source`
   - `backRef = nullptr`
   - Return.  
3. Initialize:
   - `slow = source`
   - `fast = source->next_ssd`
4. While `fast != nullptr`:
   - Move `fast` one step: `fast = fast->next_ssd`
   - If `fast != nullptr`:
     - Move `slow` one step: `slow = slow->next_ssd`
     - Move `fast` a second step: `fast = fast->next_ssd`
5. At this point:
   - `slow` points to **last node of front half**.
6. Set:
   - `*frontRef = source`
   - `*backRef = slow->next_ssd`
   - `slow->next_ssd = nullptr`  
7. Now the original list is split into two sublists.

---

## 💻 C++ Program

```cpp
#include <iostream>
using namespace std;

// ---------------- Node Structure ----------------
struct node_ssd {
    int data_ssd;
    node_ssd* next_ssd;
};

// ---------------- Function Prototypes ----------------
node_ssd* createNode_ssd(int data);
void append_ssd(node_ssd** headRef, int data);
void printList_ssd(node_ssd* head);
void FrontBackSplit_ssd(node_ssd* source, node_ssd** frontRef, node_ssd** backRef);

// ---------------- Main Function ----------------
int main() {
    node_ssd* head_ssd = nullptr;
    node_ssd* front_ssd = nullptr;
    node_ssd* back_ssd = nullptr;
    int n_ssd, val_ssd;

    cout << "Enter number of elements in the list: ";
    cin >> n_ssd;

    for(int i = 0; i < n_ssd; i++) {
        cout << "Enter element " << i+1 << ": ";
        cin >> val_ssd;
        append_ssd(&head_ssd, val_ssd);
    }

    cout << "\nOriginal List:\n";
    printList_ssd(head_ssd);

    FrontBackSplit_ssd(head_ssd, &front_ssd, &back_ssd);

    cout << "\nFront List:\n";
    printList_ssd(front_ssd);
    cout << "\nBack List:\n";
    printList_ssd(back_ssd);

    return 0;
}

// ---------------- Node Functions ----------------
node_ssd* createNode_ssd(int data) {
    node_ssd* newNode = new node_ssd;
    newNode->data_ssd = data;
    newNode->next_ssd = nullptr;
    return newNode;
}

void append_ssd(node_ssd** headRef, int data) {
    node_ssd* newNode = createNode_ssd(data);
    if(*headRef == nullptr) {
        *headRef = newNode;
        return;
    }
    node_ssd* temp = *headRef;
    while(temp->next_ssd) temp = temp->next_ssd;
    temp->next_ssd = newNode;
}

void printList_ssd(node_ssd* head) {
    if(!head) {
        cout << "List is empty\n";
        return;
    }
    node_ssd* temp = head;
    while(temp) {
        cout << temp->data_ssd << " ";
        temp = temp->next_ssd;
    }
    cout << endl;
}

// ---------------- FrontBackSplit Function ----------------
void FrontBackSplit_ssd(node_ssd* source, node_ssd** frontRef, node_ssd** backRef) {
    // Handle lists of length 0 or 1
    if(source == nullptr || source->next_ssd == nullptr) {
        *frontRef = source;
        *backRef = nullptr;
        return;
    }

    node_ssd* slow = source;
    node_ssd* fast = source->next_ssd;

    // Move fast by 2 and slow by 1
    while(fast) {
        fast = fast->next_ssd;
        if(fast) {
            slow = slow->next_ssd;
            fast = fast->next_ssd;
        }
    }

    // slow is at the end of the front list
    *frontRef = source;
    *backRef = slow->next_ssd;
    slow->next_ssd = nullptr;
}
```

---

## 🧪 Example Execution

```text
Enter number of elements in the list: 10
Enter element 1: 10
Enter element 2: 20
Enter element 3: 30
Enter element 4: 40
Enter element 5: 50
Enter element 6: 60
Enter element 7: 70
Enter element 8: 80
Enter element 9: 90
Enter element 10: 100

Original List:
10 20 30 40 50 60 70 80 90 100 

Front List:
10 20 30 40 50 

Back List:
60 70 80 90 100 
```

Program correctly splits the list into a **front half** and **back half** with equal sizes for even length.

---

## 🧠 Memory Visualization

For the input:

```text
List: 10 → 20 → 30 → 40 → 50 → 60 → 70 → 80 → 90 → 100
```

### Before Split

```text
head_ssd
  ↓
[10] → [20] → [30] → [40] → [50] → [60] → [70] → [80] → [90] → [100] → NULL
```

Pointers during `FrontBackSplit_ssd`:

- `slow` moves one step  
- `fast` moves two steps  

Final positions (for 10 elements):

- `slow` at node with **50**  
- `fast` becomes `NULL`

### After Split

**Front List (`front_ssd`)**:

```text
[10] → [20] → [30] → [40] → [50] → NULL
```

**Back List (`back_ssd`)**:

```text
[60] → [70] → [80] → [90] → [100] → NULL
```

The link between `50` and `60` is broken by:

```cpp
slow->next_ssd = nullptr;
```

---

## ✅ Conclusion

This assignment demonstrates:

- Creation and traversal of a **singly linked list**.  
- Use of the **fast–slow pointer technique** to split a list into front and back halves.  
- Correct handling of:
  - Empty list  
  - Single node list  
  - Even-length and odd-length lists  

The `FrontBackSplit_ssd` function works efficiently in **O(n)** time and **O(1)** extra space using only pointer manipulation, without extra arrays or counting the length first.

