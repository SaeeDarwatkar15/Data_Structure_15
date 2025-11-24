# 🧩 Assignment 14: Set Operations on Student Sports Preferences Using Linked Lists

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment represents **two sets of students** (Cricket and Football fans) using **singly linked lists**, and performs classic **set operations**: intersection, symmetric difference, and counting students in neither set.

---

## 📝 Problem Statement

In the Second Year Computer Engineering class:

- **Set A** → Students who like **Cricket**  
- **Set B** → Students who like **Football**

Using **linked lists** to represent these sets, write a C++ program to:

1. Find and display the set of students who like **both** Cricket and Football.  
2. Find and display the set of students who like **either** Cricket or Football, but **not both**.  
3. Display the **number of students** who like **neither** Cricket nor Football.

Students are identified using **PRN** and **Name**.

---

## 📘 Theory

### 🔹 Set Representation Using Linked Lists

Each set (A and B) is implemented as a **singly linked list**, where each node contains:

- `prn_ssd` → Unique roll number (PRN)  
- `name_ssd` → Student name  
- `next_ssd` → Pointer to next node  

A separate **master list** stores all students in the class.

### 🔹 Why Linked Lists?

- Dynamic size — no need to predefine number of elements.  
- Easy to add or traverse students belonging to any set.  
- Flexible for set operations using pointer traversal.

---

### 🔹 Master List and Sets

1. **Master List (`master`)**
   - Contains **all students** in the class.
   - PRNs are auto-assigned sequentially from `1000`.

2. **Set A (`setA`)**
   - Students who like **Cricket**.
   - User enters PRNs of Cricket fans.

3. **Set B (`setB`)**
   - Students who like **Football**.
   - User enters PRNs of Football fans.

Names are **auto-generated randomly** from a fixed list of sample names.

---

### 🔹 Set Operations

1. **Intersection (A ∩ B)**  
   Students who like **both** Cricket and Football.  
   - For each node in A, check if its PRN exists in B.

2. **Symmetric Difference (A Δ B)**  
   Students who like **either** Cricket or Football, but **not both**.  
   - (A − B) ∪ (B − A)  
   - Include a student if they are in **exactly one** of the sets.

3. **Neither (Complement)**  
   Students in the master list who are in **neither A nor B**.  
   - For each student in `master`, check if they are **not present** in both sets.

---

## 📊 Algorithm (Step-by-Step)

### 1️⃣ Create Master List

1. Input `total` = total students in class.  
2. For `i = 0` to `total - 1`:
   - Create new node with:
     - `prn_ssd = 1000 + i`
     - `name_ssd = random name`
   - Append to master linked list.

---

### 2️⃣ Build Set A (Cricket)

1. Input `nA` = number of students who like Cricket.  
2. Repeat `nA` times:
   - Read PRN.
   - Search in master list.
   - If found, copy that node into `setA`.

---

### 3️⃣ Build Set B (Football)

1. Input `nB` = number of students who like Football.  
2. Repeat `nB` times:
   - Read PRN.
   - Search in master list.
   - If found, copy that node into `setB`.

---

### 4️⃣ Intersection (A ∩ B)

1. For each node `a` in A:
   - For each node `b` in B:
     - If `a->prn_ssd == b->prn_ssd`, add `a` to `result`.

---

### 5️⃣ Symmetric Difference (A Δ B)

1. **A − B**:
   - For each `a` in A:
     - If its PRN is **not found** in B → add to result.
2. **B − A**:
   - For each `b` in B:
     - If its PRN is **not found** in A → add to result.

---

### 6️⃣ Count Students Who Like Neither

1. For each student `m` in master:
   - Check if `m` is in A.
   - Check if `m` is in B.
   - If in neither → increment `count`.

---

## 💻 C++ Program

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <string>
using namespace std;

struct Student_ssd {
    int prn_ssd;
    string name_ssd;
    Student_ssd* next_ssd;
};
typedef Student_ssd* StudentPtr_ssd;

// Generate random name from a fixed list
string generateRandomName_ssd() {
    string names[] = {
        "Aarav","Siya","Rohan","Priya","Kabir","Anaya","Vihan","Meera","Ishaan","Kavya",
        "Aditi","Arjun","Sneha","Dev","Riya","Reyansh","Neha","Krish","Tara","Manav"
    };
    int totalNames = sizeof(names) / sizeof(names[0]);
    return names[rand() % totalNames];
}

// Function to create master list with random names
StudentPtr_ssd createMasterList_ssd(int total) {
    StudentPtr_ssd head = NULL, temp = NULL;
    for (int i = 0; i < total; i++) {
        StudentPtr_ssd newNode = new Student_ssd;
        newNode->prn_ssd = 1000 + i;
        newNode->name_ssd = generateRandomName_ssd();
        newNode->next_ssd = NULL;

        if (head == NULL)
            head = newNode;
        else
            temp->next_ssd = newNode;
        temp = newNode;
    }
    return head;
}

// Print list
void printList_ssd(StudentPtr_ssd head) {
    if (!head) {
        cout << "List is empty.\n";
        return;
    }
    while (head) {
        cout << "PRN: " << head->prn_ssd << " | Name: " << head->name_ssd << endl;
        head = head->next_ssd;
    }
}

// Find student by PRN in master list
StudentPtr_ssd findStudent_ssd(StudentPtr_ssd master, int prn) {
    while (master) {
        if (master->prn_ssd == prn)
            return master;
        master = master->next_ssd;
    }
    return NULL;
}

// Add student to a set (copy node)
void addToSet_ssd(StudentPtr_ssd& set, StudentPtr_ssd student) {
    if (!student) return;

    StudentPtr_ssd newNode = new Student_ssd;
    newNode->prn_ssd = student->prn_ssd;
    newNode->name_ssd = student->name_ssd;
    newNode->next_ssd = NULL;

    if (set == NULL)
        set = newNode;
    else {
        StudentPtr_ssd temp = set;
        while (temp->next_ssd)
            temp = temp->next_ssd;
        temp->next_ssd = newNode;
    }
}

// Intersection: students in both A and B
StudentPtr_ssd intersection_ssd(StudentPtr_ssd A, StudentPtr_ssd B) {
    StudentPtr_ssd result = NULL;
    for (StudentPtr_ssd a = A; a; a = a->next_ssd) {
        for (StudentPtr_ssd b = B; b; b = b->next_ssd) {
            if (a->prn_ssd == b->prn_ssd) {
                addToSet_ssd(result, a);
            }
        }
    }
    return result;
}

// Symmetric difference: in A or B but not both
StudentPtr_ssd symDiff_ssd(StudentPtr_ssd A, StudentPtr_ssd B) {
    StudentPtr_ssd result = NULL;

    // A - B
    for (StudentPtr_ssd a = A; a; a = a->next_ssd) {
        bool found = false;
        for (StudentPtr_ssd b = B; b; b = b->next_ssd) {
            if (a->prn_ssd == b->prn_ssd) {
                found = true;
                break;
            }
        }
        if (!found)
            addToSet_ssd(result, a);
    }

    // B - A
    for (StudentPtr_ssd b = B; b; b = b->next_ssd) {
        bool found = false;
        for (StudentPtr_ssd a = A; a; a = a->next_ssd) {
            if (a->prn_ssd == b->prn_ssd) {
                found = true;
                break;
            }
        }
        if (!found)
            addToSet_ssd(result, b);
    }

    return result;
}

// Count students who like neither Cricket nor Football
int countNeither_ssd(StudentPtr_ssd master, StudentPtr_ssd A, StudentPtr_ssd B) {
    int count = 0;
    for (StudentPtr_ssd m = master; m; m = m->next_ssd) {
        bool inA = false, inB = false;

        // Check in A
        for (StudentPtr_ssd a = A; a; a = a->next_ssd) {
            if (m->prn_ssd == a->prn_ssd) {
                inA = true;
                break;
            }
        }

        // Check in B
        for (StudentPtr_ssd b = B; b; b = b->next_ssd) {
            if (m->prn_ssd == b->prn_ssd) {
                inB = true;
                break;
            }
        }

        if (!inA && !inB)
            count++;
    }
    return count;
}

// Main function
int main() {
    srand(time(0));
    int total, nA, nB;

    cout << "Enter total number of students in class: ";
    cin >> total;

    // Create master list
    StudentPtr_ssd master = createMasterList_ssd(total);

    cout << "\nAuto-generated master list:\n";
    printList_ssd(master);

    // Create Set A (Cricket)
    cout << "\nEnter number of students who like Cricket: ";
    cin >> nA;
    StudentPtr_ssd setA = NULL;
    for (int i = 0; i < nA; i++) {
        int prn;
        cout << "Enter PRN of Cricket fan: ";
        cin >> prn;
        StudentPtr_ssd s = findStudent_ssd(master, prn);
        if (s)
            addToSet_ssd(setA, s);
        else
            cout << "PRN not found in master list!\n";
    }

    // Create Set B (Football)
    cout << "\nEnter number of students who like Football: ";
    cin >> nB;
    StudentPtr_ssd setB = NULL;
    for (int i = 0; i < nB; i++) {
        int prn;
        cout << "Enter PRN of Football fan: ";
        cin >> prn;
        StudentPtr_ssd s = findStudent_ssd(master, prn);
        if (s)
            addToSet_ssd(setB, s);
        else
            cout << "PRN not found in master list!\n";
    }

    // Perform set operations
    StudentPtr_ssd inter = intersection_ssd(setA, setB);
    StudentPtr_ssd sym = symDiff_ssd(setA, setB);
    int neither = countNeither_ssd(master, setA, setB);

    cout << "\n--- Students who like BOTH Cricket and Football ---\n";
    printList_ssd(inter);

    cout << "\n--- Students who like EITHER Cricket or Football but NOT both ---\n";
    printList_ssd(sym);

    cout << "\nNumber of students who like NEITHER Cricket nor Football: "
         << neither << endl;

    return 0;
}
```

---

## 🧪 Example Execution

```text
Enter total number of students in class: 10

Auto-generated master list:
PRN: 1000 | Name: Sneha
PRN: 1001 | Name: Aditi
PRN: 1002 | Name: Meera
PRN: 1003 | Name: Tara
PRN: 1004 | Name: Neha
PRN: 1005 | Name: Tara
PRN: 1006 | Name: Kabir
PRN: 1007 | Name: Sneha
PRN: 1008 | Name: Sneha
PRN: 1009 | Name: Tara

Enter number of students who like Cricket: 8
Enter PRN of Cricket fan: 1000
Enter PRN of Cricket fan: 1001
Enter PRN of Cricket fan: 1009
Enter PRN of Cricket fan: 1003
Enter PRN of Cricket fan: 1004
Enter PRN of Cricket fan: 1002
Enter PRN of Cricket fan: 1006
Enter PRN of Cricket fan: 1008

Enter number of students who like Football: 5
Enter PRN of Football fan: 1000
Enter PRN of Football fan: 1008
Enter PRN of Football fan: 1004
Enter PRN of Football fan: 1001
Enter PRN of Football fan: 1009

--- Students who like BOTH Cricket and Football ---
PRN: 1000 | Name: Sneha
PRN: 1001 | Name: Aditi
PRN: 1009 | Name: Tara
PRN: 1004 | Name: Neha
PRN: 1008 | Name: Sneha

--- Students who like EITHER Cricket or Football but NOT both ---
PRN: 1003 | Name: Tara
PRN: 1002 | Name: Meera
PRN: 1006 | Name: Kabir

Number of students who like NEITHER Cricket nor Football: 2
```

---

## 🧠 Memory Visualization

### Linked Lists in Memory

- **Master List (`master`)**: all students  
- **Set A (`setA`)**: Cricket fans  
- **Set B (`setB`)**: Football fans  
- **Intersection (`inter`)**: A ∩ B  
- **Symmetric Difference (`sym`)**: A Δ B  

Each node:

| Field       | Description                  |
|------------|------------------------------|
| `prn_ssd`  | Unique student PRN           |
| `name_ssd` | Student name                 |
| `next_ssd` | Pointer to next student node |

Example (partial):

```text
master:
[1000, Sneha] -> [1001, Aditi] -> [1002, Meera] -> ... -> NULL

setA (Cricket):
[1000] -> [1001] -> [1009] -> [1003] -> ...

setB (Football):
[1000] -> [1008] -> [1004] -> [1001] -> [1009] -> ...
```

---

## ✅ Conclusion

This assignment demonstrates:

- How to represent **sets using linked lists** in C++.  
- Implementation of **intersection**, **symmetric difference**, and **complement (neither)** using pointer-based traversal.  
- Use of a **master list** to validate PRNs and compute accurate counts.  

It strengthens understanding of **set theory**, **linked lists**, and **basic searching/traversal** operations in data structures.

