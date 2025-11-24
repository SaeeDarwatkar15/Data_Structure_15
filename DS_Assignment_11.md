# 🧩 Assignment 11: Singly Linked List for ‘Vertex Club’ Membership Management

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment implements a **Singly Linked List** to manage **Vertex Club** membership records, including **President**, **Members**, and **Secretary**, with common operations like insertion, deletion, counting, reversing, sorting, etc.

---

## 1️⃣ Problem Statement

The Department of Computer Engineering has a student club named **‘Vertex Club’** for **S.Y., T.Y., and B.Tech** students.  

- The **first member** in the list is the **President**.  
- The **last member** in the list is the **Secretary**.  

Write a C++ program using a **Singly Linked List** to:

- Add / Delete members (including **President** / **Secretary**)  
- Count total members  
- Display member list  
- Concatenate two division lists  
- Reverse the list  
- Search a member by PRN  
- Sort the list by PRN  

---

## 2️⃣ Theory

### 🔹 Linked List Concept

A **Linked List** is a linear data structure where each element (called a **node**) contains:

1. **Data** → PRN, Name, Designation  
2. **Pointer** → Address of the next node  

In a **Singly Linked List**, each node points **only to the next** node.

### 🔹 Node Structure in This Program

Each node represents a **Vertex Club member** with:

- `prn_ssd` → Unique PRN of the student  
- `name_ssd` → Name of the member  
- `designation_ssd` → "President", "Member", or "Secretary"  
- `next_ssd` → Pointer to the next node  

### 🔹 Major Operations

1. **Node Creation**  
   - Created dynamically using `new` → allows flexible size, no fixed array limit.

2. **Insertion**
   - **President** → Insert at the **beginning** of the list.  
   - **Secretary** → Insert at the **end** of the list.  
   - **Member** → Insert **between** President and Secretary.

3. **Deletion**
   - Delete a node based on its **PRN**.
   - Handles deletion of:
     - First node (President)
     - Middle Member
     - Last node (Secretary)

4. **Counting**
   - Traverse the list and count number of nodes.

5. **Display**
   - Print all member details in order.

6. **Reversal**
   - Reverse the list by changing `next` pointers of nodes.

7. **Sorting**
   - Sort members by **PRN** using a simple **Bubble Sort-like** approach over the linked list.

8. **Concatenation**
   - Join two division lists (Div A and Div B) into one list.

9. **Destruction**
   - Free all dynamically allocated nodes before exiting to avoid memory leaks.

---

## 3️⃣ Algorithm (Step-by-Step)

### Node & List Setup
1. Define `struct Member_ssd` with:
   - `int prn_ssd`
   - `char name_ssd[50]`
   - `char designation_ssd[20]`
   - `Member_ssd* next_ssd`

2. Use `Member_ssd*` head pointer (e.g., `divA_ssd`, `divB_ssd`) to refer to start of each list.

---

### Insertion Operations

#### 🔸 Insert President
1. Create a new node.  
2. Input PRN and Name.  
3. Set designation = `"President"`.  
4. Point `newNode->next` to current `head`.  
5. Make `head = newNode`.

#### 🔸 Insert Secretary
1. Create a new node, set designation `"Secretary"`.  
2. If list empty → return new node as head.  
3. Else traverse to last node.  
4. Set last node’s `next` to new node.

#### 🔸 Insert Member
1. Create a new node, set designation `"Member"`.  
2. If list is empty → member becomes first node.  
3. Else traverse list **until before Secretary**.  
4. Insert new node before Secretary by adjusting links.

---

### Deletion by PRN

1. If list is empty → nothing to delete.  
2. If head node’s PRN matches → delete head, move head to `head->next`.  
3. Else traverse list while tracking previous node.  
4. When node with matching PRN is found:
   - Bypass it: `prev->next = temp->next`.  
   - Delete that node.

---

### Counting Members

1. Start from head.  
2. Traverse using `next` until `NULL`.  
3. Increment a counter for each node.  
4. Return count.

---

### Reversing the List

1. Use three pointers: `prev`, `curr`, `next`.  
2. Initially: `prev = NULL`, `curr = head`.  
3. For each node:
   - Save `next = curr->next`.  
   - Point `curr->next = prev`.  
   - Move `prev = curr`, `curr = next`.  
4. At end, `prev` becomes new head.

---

### Sorting by PRN

1. Use **nested loops** (Bubble Sort-like).  
2. For every node `i`, compare with node `j` after it.  
3. If `i->prn > j->prn`, **swap the data fields only** (PRN, name, designation).  

---

### Concatenation of Two Lists

1. If first list is empty → return second list.  
2. Traverse to end of first list.  
3. Set last node’s `next` to head of second list.  

---

## 4️⃣ C++ Program

```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Member_ssd {
    int prn_ssd;
    char name_ssd[50];
    char designation_ssd[20];
    Member_ssd* next_ssd;
};

Member_ssd* getNode_ssd() {
    Member_ssd* newNode_ssd = new Member_ssd;
    newNode_ssd->next_ssd = NULL;
    return newNode_ssd;
}

Member_ssd* insertPresident_ssd(Member_ssd* head_ssd) {
    Member_ssd* newNode_ssd = getNode_ssd();
    cout << "Enter PRN: ";
    cin >> newNode_ssd->prn_ssd;
    cout << "Enter Name: ";
    cin >> newNode_ssd->name_ssd;
    strcpy(newNode_ssd->designation_ssd, "President");
    newNode_ssd->next_ssd = head_ssd;
    return newNode_ssd;
}

Member_ssd* insertSecretary_ssd(Member_ssd* head_ssd) {
    Member_ssd* newNode_ssd = getNode_ssd();
    cout << "Enter PRN: ";
    cin >> newNode_ssd->prn_ssd;
    cout << "Enter Name: ";
    cin >> newNode_ssd->name_ssd;
    strcpy(newNode_ssd->designation_ssd, "Secretary");

    if (head_ssd == NULL)
        return newNode_ssd;

    Member_ssd* temp_ssd = head_ssd;
    while (temp_ssd->next_ssd != NULL)
        temp_ssd = temp_ssd->next_ssd;

    temp_ssd->next_ssd = newNode_ssd;
    return head_ssd;
}

Member_ssd* insertMember_ssd(Member_ssd* head_ssd) {
    Member_ssd* newNode_ssd = getNode_ssd();
    cout << "Enter PRN: ";
    cin >> newNode_ssd->prn_ssd;
    cout << "Enter Name: ";
    cin >> newNode_ssd->name_ssd;
    strcpy(newNode_ssd->designation_ssd, "Member");

    if (head_ssd == NULL)
        return newNode_ssd;

    Member_ssd* temp_ssd = head_ssd;

    // Move till just before Secretary (or end)
    while (temp_ssd->next_ssd != NULL &&
           strcmp(temp_ssd->next_ssd->designation_ssd, "Secretary") != 0)
    {
        temp_ssd = temp_ssd->next_ssd;
    }

    newNode_ssd->next_ssd = temp_ssd->next_ssd;
    temp_ssd->next_ssd = newNode_ssd;
    return head_ssd;
}

void displayList_ssd(Member_ssd* head_ssd) {
    if (!head_ssd) {
        cout << "List is empty.\n";
        return;
    }
    cout << "\n--- Vertex Club Members ---\n";
    while (head_ssd) {
        cout << "PRN: " << head_ssd->prn_ssd
             << " | Name: " << head_ssd->name_ssd
             << " | Designation: " << head_ssd->designation_ssd << endl;
        head_ssd = head_ssd->next_ssd;
    }
}

int countMembers_ssd(Member_ssd* head_ssd) {
    int count_ssd = 0;
    while (head_ssd) {
        count_ssd++;
        head_ssd = head_ssd->next_ssd;
    }
    return count_ssd;
}

Member_ssd* deleteByPRN_ssd(Member_ssd* head_ssd, int prn_ssd) {
    if (!head_ssd) return NULL;

    // If node to delete is head
    if (head_ssd->prn_ssd == prn_ssd) {
        Member_ssd* temp_ssd = head_ssd;
        head_ssd = head_ssd->next_ssd;
        delete temp_ssd;
        return head_ssd;
    }

    Member_ssd* temp_ssd = head_ssd;
    Member_ssd* prev_ssd = NULL;

    while (temp_ssd && temp_ssd->prn_ssd != prn_ssd) {
        prev_ssd = temp_ssd;
        temp_ssd = temp_ssd->next_ssd;
    }

    if (temp_ssd) {
        prev_ssd->next_ssd = temp_ssd->next_ssd;
        delete temp_ssd;
    } else {
        cout << "PRN not found.\n";
    }

    return head_ssd;
}

Member_ssd* reverseList_ssd(Member_ssd* head_ssd) {
    Member_ssd *prev_ssd = NULL, *curr_ssd = head_ssd, *next_ssd = NULL;
    while (curr_ssd) {
        next_ssd = curr_ssd->next_ssd;
        curr_ssd->next_ssd = prev_ssd;
        prev_ssd = curr_ssd;
        curr_ssd = next_ssd;
    }
    return prev_ssd;
}

void sortByPRN_ssd(Member_ssd* head_ssd) {
    for (Member_ssd* i = head_ssd; i != NULL; i = i->next_ssd) {
        for (Member_ssd* j = i->next_ssd; j != NULL; j = j->next_ssd) {
            if (i->prn_ssd > j->prn_ssd) {
                // Swap data (not links)
                int tempPrn = i->prn_ssd;
                i->prn_ssd = j->prn_ssd;
                j->prn_ssd = tempPrn;

                char tempName[50];
                strcpy(tempName, i->name_ssd);
                strcpy(i->name_ssd, j->name_ssd);
                strcpy(j->name_ssd, tempName);

                char tempDesg[20];
                strcpy(tempDesg, i->designation_ssd);
                strcpy(i->designation_ssd, j->designation_ssd);
                strcpy(j->designation_ssd, tempDesg);
            }
        }
    }
}

Member_ssd* concatenate_ssd(Member_ssd* head1_ssd, Member_ssd* head2_ssd) {
    if (!head1_ssd) return head2_ssd;
    Member_ssd* temp_ssd = head1_ssd;
    while (temp_ssd->next_ssd != NULL)
        temp_ssd = temp_ssd->next_ssd;
    temp_ssd->next_ssd = head2_ssd;
    return head1_ssd;
}

// Optional: Search member by PRN
Member_ssd* searchByPRN_ssd(Member_ssd* head_ssd, int prn_ssd) {
    while (head_ssd) {
        if (head_ssd->prn_ssd == prn_ssd)
            return head_ssd;
        head_ssd = head_ssd->next_ssd;
    }
    return NULL;
}

int main() {
    Member_ssd* divA_ssd = NULL;
    Member_ssd* divB_ssd = NULL; // can be used for concatenation if needed
    int choice_ssd, prn_ssd;

    while (true) {
        cout << "\n===== Vertex Club Menu =====\n";
        cout << "1. Add President\n";
        cout << "2. Add Member\n";
        cout << "3. Add Secretary\n";
        cout << "4. Display List\n";
        cout << "5. Delete Member by PRN\n";
        cout << "6. Count Members\n";
        cout << "7. Reverse List\n";
        cout << "8. Sort by PRN\n";
        cout << "9. Exit\n";
        cout << "Enter choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1:
                divA_ssd = insertPresident_ssd(divA_ssd);
                break;

            case 2:
                divA_ssd = insertMember_ssd(divA_ssd);
                break;

            case 3:
                divA_ssd = insertSecretary_ssd(divA_ssd);
                break;

            case 4:
                displayList_ssd(divA_ssd);
                break;

            case 5:
                cout << "Enter PRN to delete: ";
                cin >> prn_ssd;
                divA_ssd = deleteByPRN_ssd(divA_ssd, prn_ssd);
                break;

            case 6:
                cout << "Total Members: " << countMembers_ssd(divA_ssd) << endl;
                break;

            case 7:
                divA_ssd = reverseList_ssd(divA_ssd);
                cout << "List reversed.\n";
                break;

            case 8:
                sortByPRN_ssd(divA_ssd);
                cout << "List sorted by PRN.\n";
                break;

            case 9:
                cout << "Exiting...\n";
                return 0;

            default:
                cout << "Invalid choice.\n";
        }
    }
}
```

---

## 🧮 Example Execution

```text
===== Vertex Club Menu =====
1. Add President
2. Add Member
3. Add Secretary
4. Display List
5. Delete Member
6. Count Members
7. Reverse List
8. Sort by PRN
9. Exit
Enter choice: 1
Enter PRN: 121
Enter Name: Saee

===== Vertex Club Menu =====
Enter choice: 2
Enter PRN: 111
Enter Name: Jay

===== Vertex Club Menu =====
Enter choice: 3
Enter PRN: 100
Enter Name: Vijay

===== Vertex Club Menu =====
Enter choice: 4

--- Vertex Club Members ---
PRN: 121 | Name: Saee | Designation: President
PRN: 111 | Name: Jay  | Designation: Member
PRN: 100 | Name: Vijay | Designation: Secretary

===== Vertex Club Menu =====
Enter choice: 6
Total Members: 3

===== Vertex Club Menu =====
Enter choice: 7
List reversed.

===== Vertex Club Menu =====
Enter choice: 4

--- Vertex Club Members ---
PRN: 100 | Name: Vijay | Designation: Secretary
PRN: 111 | Name: Jay   | Designation: Member
PRN: 121 | Name: Saee  | Designation: President

===== Vertex Club Menu =====
Enter choice: 8
List sorted by PRN.

===== Vertex Club Menu =====
Enter choice: 4

--- Vertex Club Members ---
PRN: 100 | Name: Vijay | Designation: Secretary
PRN: 111 | Name: Jay   | Designation: Member
PRN: 121 | Name: Saee  | Designation: President

===== Vertex Club Menu =====
Enter choice: 5
Enter PRN to delete: 111

===== Vertex Club Menu =====
Enter choice: 4

--- Vertex Club Members ---
PRN: 100 | Name: Vijay | Designation: Secretary
PRN: 121 | Name: Saee  | Designation: President

===== Vertex Club Menu =====
Enter choice: 9
Exiting...
```

---

## 🧠 Memory Visualization

### Node Structure in Memory

Each node (Member_ssd) in heap:

| Field           | Description                       |
|----------------|-----------------------------------|
| `prn_ssd`      | PRN of member                     |
| `name_ssd`     | Name of member                    |
| `designation_ssd` | "President" / "Member" / "Secretary" |
| `next_ssd`     | Pointer to next node              |

### Example Linked List Layout

For list:

- 121 (President) → 111 (Member) → 100 (Secretary)

It looks like:

```text
[PRN:121 | Saee | President | *-] --> [PRN:111 | Jay | Member | *-] --> [PRN:100 | Vijay | Secretary | NULL]
```

Each `*->` is the `next_ssd` pointer to the next node.

---

## ✅ Conclusion

This assignment demonstrates:

- Practical use of **Singly Linked List** to manage club membership.
- Implementation of key operations:
  - Insertion (President, Member, Secretary)
  - Deletion by PRN
  - Counting members
  - Reversing list
  - Sorting by PRN
  - (With support functions for concatenation/search)
- Proper use of **dynamic memory allocation** in C++.

This forms a strong base for understanding **linked list operations**, **pointer manipulation**, and real-world modeling of club/member data structures.

