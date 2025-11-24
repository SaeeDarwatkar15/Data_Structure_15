# 🧮 Assignment 19: Doubly Linked List – Insertion and Deletion (All Cases)

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
Write a C++ program to **create a doubly linked list of students** and perform **all insertion and deletion operations**, including:

1. Insert at beginning  
2. Insert at end  
3. Insert after a given roll number  
4. Delete at beginning  
5. Delete at end  
6. Delete by roll number  
7. Display list from **Head → Tail** and **Tail → Head**

---

## 📝 Problem Statement

Design and implement a **menu-driven C++ program** using a **doubly linked list** to store student records with:

- Roll Number (auto-generated)  
- Name  
- Marks  

The program must support:

- **Insertion** in all cases:
  - At the beginning
  - At the end
  - After a given roll number  

- **Deletion** in all cases:
  - At the beginning
  - At the end
  - By a given roll number  

- Display:
  - List from **head to tail**
  - List from **tail to head**

---

## 📘 Theory

### 🔹 Doubly Linked List

A **doubly linked list** is a linear data structure where each node contains:

- `data` (here: roll number, name, marks)  
- Pointer to the **previous node** (`prev_ssd`)  
- Pointer to the **next node** (`next_ssd`)

Advantages:

- Traversal possible in **both directions**.  
- Insertion and deletion at **both ends** and at **any position** is easier than in arrays.

---

### 🔹 Operations Implemented

1. **Insertion at Beginning**
   - Create a new node.
   - Set `newnode->next = head`.
   - Update old head’s `prev` to new node.
   - Make new node the new head.

2. **Insertion at End**
   - Traverse to the last node.
   - Attach new node after it.
   - Set new node’s `prev` to last node.

3. **Insertion After Specific Roll**
   - Search node with given roll.
   - Link new node **after** that node:
     - `new->next = target->next`
     - `new->prev = target`
     - Fix adjacent pointers.

4. **Deletion at Beginning**
   - Move head to `head->next`.
   - Update new head’s `prev` to `NULL`.
   - Delete old head.

5. **Deletion at End**
   - Traverse to last node.
   - Adjust previous node’s `next` to `NULL`.
   - Delete last node.

6. **Deletion by Roll Number**
   - Find node whose `roll_no` matches.
   - Relink its `prev` and `next` neighbors.
   - If it’s head, update head.
   - Delete that node.

7. **Forward and Reverse Display**
   - Forward: traverse using `next_ssd`.  
   - Reverse: go to tail, then traverse using `prev_ssd`.

---

## 🧠 Algorithm (Step-by-Step)

### A) Insertion

#### 1️⃣ Insert at Beginning
1. Create new node with name, marks, and auto-roll.  
2. Set `new->next = head`.  
3. If `head != NULL` → `head->prev = new`.  
4. Update `head = new`.

#### 2️⃣ Insert at End
1. Create new node.  
2. If `head == NULL` → new node becomes head.  
3. Else, traverse to last node.  
4. Set `last->next = new` and `new->prev = last`.

#### 3️⃣ Insert After Specific Roll
1. Input `targetRoll`.  
2. Search list until `node->roll == targetRoll`.  
3. If not found → print message.  
4. Else:
   - Create new node.
   - Link between `target` and `target->next`.

---

### B) Deletion

#### 4️⃣ Delete at Beginning
1. If list empty → print message.  
2. Move `head = head->next`.  
3. Set `head->prev = NULL` (if not NULL).  
4. Delete old head.

#### 5️⃣ Delete at End
1. If list empty → print message.  
2. If single node → delete and set `head = NULL`.  
3. Else:
   - Traverse to last node.
   - `last->prev->next = NULL`.
   - Delete last node.

#### 6️⃣ Delete by Roll No
1. Search for node with matching roll.  
2. If not found → print error.  
3. If node is head:
   - `head = head->next`
   - Fix `head->prev`
3. Else:
   - `prev->next = current->next`
   - If `current->next` exists → fix its `prev`.  
4. Delete current node.

---

### C) Display

#### 7️⃣ Print From Head to Tail
- Start from `head`, follow `next_ssd`, print each node.

#### 8️⃣ Print From Tail to Head
- Traverse to last node.  
- Follow `prev_ssd` backward, printing each node.

---

## 💻 C++ Program

```cpp
#include <iostream>
#include <string>
using namespace std;

// ---------------- Node Structure ----------------
struct student_ssd {
    int roll_no_ssd;
    string name_ssd;
    float marks_ssd;
    student_ssd* prev_ssd;
    student_ssd* next_ssd;
};

static int rollCounter_ssd = 1;

// ---------------- Function Prototypes ----------------
student_ssd* getNode_ssd();
student_ssd* insertAtBeginning_ssd(student_ssd* head);
student_ssd* insertAtEnd_ssd(student_ssd* head);
student_ssd* insertAfterRoll_ssd(student_ssd* head, int targetRoll);

student_ssd* deleteAtBeginning_ssd(student_ssd* head);
student_ssd* deleteAtEnd_ssd(student_ssd* head);
student_ssd* deleteByRoll_ssd(student_ssd* head, int roll);

void printList_ssd(student_ssd* head);
void printListReverse_ssd(student_ssd* head);

// ---------------- Menu Function ----------------
int menu_ssd() {
    int choice;
    cout << "\n========== Doubly Linked List Menu ==========\n";
    cout << "1. Insert at Beginning\n";
    cout << "2. Insert at End\n";
    cout << "3. Insert After Specific Roll No\n";
    cout << "4. Delete at Beginning\n";
    cout << "5. Delete at End\n";
    cout << "6. Delete by Roll No\n";
    cout << "7. Print List (Head -> Tail)\n";
    cout << "8. Print List (Tail -> Head)\n";
    cout << "9. Exit\n";
    cout << "Enter your choice: ";
    cin >> choice;
    return choice;
}

// ---------------- Main Function ----------------
int main() {
    student_ssd* head_ssd = nullptr;
    int choice_ssd, roll_ssd;

    while(true) {
        choice_ssd = menu_ssd();
        switch(choice_ssd) {
            case 1: head_ssd = insertAtBeginning_ssd(head_ssd); break;
            case 2: head_ssd = insertAtEnd_ssd(head_ssd); break;
            case 3:
                cout << "Enter roll no after which to insert: ";
                cin >> roll_ssd;
                head_ssd = insertAfterRoll_ssd(head_ssd, roll_ssd);
                break;
            case 4: head_ssd = deleteAtBeginning_ssd(head_ssd); break;
            case 5: head_ssd = deleteAtEnd_ssd(head_ssd); break;
            case 6:
                cout << "Enter roll no to delete: ";
                cin >> roll_ssd;
                head_ssd = deleteByRoll_ssd(head_ssd, roll_ssd); break;
            case 7: printList_ssd(head_ssd); break;
            case 8: printListReverse_ssd(head_ssd); break;
            case 9: cout << "Exiting...\n"; return 0;
            default: cout << "Invalid choice.\n";
        }
    }
    return 0;
}

// ---------------- Helper Functions ----------------
student_ssd* getNode_ssd() {
    student_ssd* newnode_ssd = new student_ssd;
    newnode_ssd->prev_ssd = nullptr;
    newnode_ssd->next_ssd = nullptr;
    newnode_ssd->roll_no_ssd = rollCounter_ssd++;
    cout << "Enter name: ";
    cin >> newnode_ssd->name_ssd;
    cout << "Enter marks: ";
    cin >> newnode_ssd->marks_ssd;
    return newnode_ssd;
}

// Insert at Beginning
student_ssd* insertAtBeginning_ssd(student_ssd* head_ssd) {
    student_ssd* newnode_ssd = getNode_ssd();
    newnode_ssd->next_ssd = head_ssd;
    if(head_ssd) head_ssd->prev_ssd = newnode_ssd;
    return newnode_ssd;
}

// Insert at End
student_ssd* insertAtEnd_ssd(student_ssd* head_ssd) {
    student_ssd* newnode_ssd = getNode_ssd();
    if(!head_ssd) return newnode_ssd;
    student_ssd* temp_ssd = head_ssd;
    while(temp_ssd->next_ssd) temp_ssd = temp_ssd->next_ssd;
    temp_ssd->next_ssd = newnode_ssd;
    newnode_ssd->prev_ssd = temp_ssd;
    return head_ssd;
}

// Insert After Specific Roll No
student_ssd* insertAfterRoll_ssd(student_ssd* head_ssd, int targetRoll) {
    student_ssd* temp_ssd = head_ssd;
    while(temp_ssd && temp_ssd->roll_no_ssd != targetRoll) temp_ssd = temp_ssd->next_ssd;
    if(!temp_ssd) { cout << "Roll no " << targetRoll << " not found.\n"; return head_ssd; }

    student_ssd* newnode_ssd = getNode_ssd();
    newnode_ssd->next_ssd = temp_ssd->next_ssd;
    newnode_ssd->prev_ssd = temp_ssd;
    if(temp_ssd->next_ssd) temp_ssd->next_ssd->prev_ssd = newnode_ssd;
    temp_ssd->next_ssd = newnode_ssd;
    return head_ssd;
}

// Delete at Beginning
student_ssd* deleteAtBeginning_ssd(student_ssd* head_ssd) {
    if(!head_ssd) { cout << "List is empty\n"; return nullptr; }
    student_ssd* temp_ssd = head_ssd;
    head_ssd = head_ssd->next_ssd;
    if(head_ssd) head_ssd->prev_ssd = nullptr;
    delete temp_ssd;
    return head_ssd;
}

// Delete at End
student_ssd* deleteAtEnd_ssd(student_ssd* head_ssd) {
    if(!head_ssd) { cout << "List is empty\n"; return nullptr; }
    if(!head_ssd->next_ssd) { delete head_ssd; return nullptr; }
    student_ssd* temp_ssd = head_ssd;
    while(temp_ssd->next_ssd) temp_ssd = temp_ssd->next_ssd;
    temp_ssd->prev_ssd->next_ssd = nullptr;
    delete temp_ssd;
    return head_ssd;
}

// Delete by Roll No
student_ssd* deleteByRoll_ssd(student_ssd* head_ssd, int roll_ssd) {
    student_ssd* temp_ssd = head_ssd;
    while(temp_ssd && temp_ssd->roll_no_ssd != roll_ssd) temp_ssd = temp_ssd->next_ssd;
    if(!temp_ssd) { cout << "Roll no " << roll_ssd << " not found\n"; return head_ssd; }
    if(temp_ssd->prev_ssd) temp_ssd->prev_ssd->next_ssd = temp_ssd->next_ssd;
    else head_ssd = temp_ssd->next_ssd;
    if(temp_ssd->next_ssd) temp_ssd->next_ssd->prev_ssd = temp_ssd->prev_ssd;
    delete temp_ssd;
    return head_ssd;
}

// Print List Head -> Tail
void printList_ssd(student_ssd* head_ssd) {
    if(!head_ssd) { cout << "List is empty\n"; return; }
    cout << "\nList (Head -> Tail):\n";
    while(head_ssd) {
        cout << "Roll No: " << head_ssd->roll_no_ssd 
             << " | Name: " << head_ssd->name_ssd 
             << " | Marks: " << head_ssd->marks_ssd << endl;
        head_ssd = head_ssd->next_ssd;
    }
}

// Print List Tail -> Head
void printListReverse_ssd(student_ssd* head_ssd) {
    if(!head_ssd) { cout << "List is empty\n"; return; }
    while(head_ssd->next_ssd) head_ssd = head_ssd->next_ssd; // go to tail
    cout << "\nList (Tail -> Head):\n";
    while(head_ssd) {
        cout << "Roll No: " << head_ssd->roll_no_ssd 
             << " | Name: " << head_ssd->name_ssd 
             << " | Marks: " << head_ssd->marks_ssd << endl;
        head_ssd = head_ssd->prev_ssd;
    }
}
```

---

## 🧪 Example Execution

```text
========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 1
Enter name: saee
Enter marks: 90

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 2
Enter name: jay
Enter marks: 98

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 1
Enter name: vijay
Enter marks: 77

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 3
Enter roll no after which to insert: 1
Enter name: Ajay
Enter marks: 88

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 7

List (Head -> Tail):
Roll No: 3 | Name: vijay | Marks: 77
Roll No: 1 | Name: saee | Marks: 90
Roll No: 4 | Name: Ajay | Marks: 88
Roll No: 2 | Name: jay | Marks: 98

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 8

List (Tail -> Head):
Roll No: 2 | Name: jay | Marks: 98
Roll No: 4 | Name: Ajay | Marks: 88
Roll No: 1 | Name: saee | Marks: 90
Roll No: 3 | Name: vijay | Marks: 77

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 4

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 5

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 6
Enter roll no to delete: 3
Roll no 3 not found

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 7

List (Head -> Tail):
Roll No: 1 | Name: saee | Marks: 90
Roll No: 4 | Name: Ajay | Marks: 88

========== Doubly Linked List Menu ==========
1. Insert at Beginning
2. Insert at End
3. Insert After Specific Roll No
4. Delete at Beginning
5. Delete at End
6. Delete by Roll No
7. Print List (Head -> Tail)
8. Print List (Tail -> Head)
9. Exit
Enter your choice: 9
Exiting...
```

---

## 🧠 Memory Visualization

Each node of the doubly linked list:

| Field            | Description                 |
|------------------|-----------------------------|
| `roll_no_ssd`    | Auto-generated roll number  |
| `name_ssd`       | Student name                |
| `marks_ssd`      | Student marks               |
| `prev_ssd`       | Pointer to previous node    |
| `next_ssd`       | Pointer to next node        |

Example (after some insertions):

```text
[3: vijay] <-> [1: saee] <-> [4: Ajay] <-> [2: jay] -> NULL
```

Backward:

```text
NULL <- [3: vijay] <- [1: saee] <- [4: Ajay] <- [2: jay]
```

---

## ✅ Conclusion

This assignment successfully:

- Implements a **doubly linked list** of student records.
- Supports **all insertion and deletion cases**:
  - At beginning, at end, after specific node, and by roll number.
- Demonstrates **forward and reverse traversal**.
- Strengthens understanding of pointer manipulation and dynamic memory usage in C++.

