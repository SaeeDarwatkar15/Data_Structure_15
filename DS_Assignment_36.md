# 🧩 Assignment 36: **Assigning Roll Numbers Using Binary Search Tree (BST)**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  

This assignment uses a **Binary Search Tree (BST)** to **assign roll numbers** to students based on their **previous year marks**. The **topper gets roll no. 1**, the second ranker gets roll no. 2, and so on.

---

## 📝 Problem Statement

Write a C++ program that:  
- Stores student information using a **tree structure**:  
  - **PRN**  
  - **Name**  
  - **Marks**  
  - **Roll number** (to be assigned)  
- Inserts students into a **BST ordered by marks** (and PRN for tie-breaking).  
- Assigns **roll numbers** such that:  
  - Highest marks → **Roll 1**  
  - Next highest → **Roll 2**  
  - And so on…  
- Displays students with their **assigned roll numbers**.

---

## 📘 Theory

### 🔹 Why Use a Binary Search Tree (BST)?

A **BST** allows us to store data so that:
- **Left subtree** of a node contains students with **lower marks**
- **Right subtree** contains students with **higher marks**
- If marks are **equal**, we break ties using **PRN (lexicographical order)**

This makes it easy to traverse students **in order of marks**.

---

### 🔹 Roll Number Assignment Logic

We want:
- Highest marks → smallest roll number (**1**)

We build a BST by **marks**, then:
- Perform a **reverse inorder traversal**:  
  **Right → Node → Left**

This visits students from **highest marks to lowest**.  
During this traversal:
- Use a counter `counter_ssd` starting from **1**  
- For each visited node (student):  
  - Assign `roll_ssd = counter_ssd`  
  - Increment `counter_ssd`  

So:
- 1st visited (highest marks) → roll 1  
- 2nd visited → roll 2  
- etc.

---

### 🔹 Handling Ties (Same Marks)

When two students have the **same marks**:
- Compare their **PRN** strings  
- Lower PRN (lexicographically) goes to **left**  
- Higher PRN goes to **right**

This makes the ordering **consistent and deterministic**.

---

## 💻 CODE 

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Student_ssd {
    string prn_ssd;
    string name_ssd;
    int marks_ssd;
    int roll_ssd; // assigned later; lower number = better rank
    Student_ssd *left_ssd, *right_ssd;

    Student_ssd(const string &prn, const string &name, int marks) {
        prn_ssd = prn;
        name_ssd = name;
        marks_ssd = marks;
        roll_ssd = 0;
        left_ssd = right_ssd = nullptr;
    }
};

// Insert by (marks, prn) - smaller goes left, larger goes right
Student_ssd* insert_ssd(Student_ssd* root_ssd, Student_ssd* node_ssd) {
    if (!root_ssd) return node_ssd;

    if (node_ssd->marks_ssd < root_ssd->marks_ssd) {
        root_ssd->left_ssd = insert_ssd(root_ssd->left_ssd, node_ssd);
    } else if (node_ssd->marks_ssd > root_ssd->marks_ssd) {
        root_ssd->right_ssd = insert_ssd(root_ssd->right_ssd, node_ssd);
    } else {
        // tie - compare PRN lexicographically
        if (node_ssd->prn_ssd < root_ssd->prn_ssd)
            root_ssd->left_ssd = insert_ssd(root_ssd->left_ssd, node_ssd);
        else
            root_ssd->right_ssd = insert_ssd(root_ssd->right_ssd, node_ssd);
    }
    return root_ssd;
}

// Assign rolls using reverse inorder: right -> node -> left
void assignRollsReverseInorder_ssd(Student_ssd* root_ssd, int &counter_ssd) {
    if (!root_ssd) return;

    assignRollsReverseInorder_ssd(root_ssd->right_ssd, counter_ssd);
    root_ssd->roll_ssd = counter_ssd++;
    assignRollsReverseInorder_ssd(root_ssd->left_ssd, counter_ssd);
}

// Display students (inorder: ascending marks) but showing assigned rolls
void displayWithRolls_ssd(Student_ssd* root_ssd) {
    if (!root_ssd) return;

    displayWithRolls_ssd(root_ssd->left_ssd);
    cout << "Roll: " << root_ssd->roll_ssd
         << " | PRN: " << root_ssd->prn_ssd
         << " | Name: " << root_ssd->name_ssd
         << " | Marks: " << root_ssd->marks_ssd << "\n";
    displayWithRolls_ssd(root_ssd->right_ssd);
}

// Utility: print tree (simple sideways view) for debugging
void printTree_ssd(Student_ssd* root_ssd, int space_ssd = 0, int indent_ssd = 6) {
    if (!root_ssd) return;

    space_ssd += indent_ssd;
    printTree_ssd(root_ssd->right_ssd, space_ssd, indent_ssd);

    cout << endl;
    for (int i_ssd = indent_ssd; i_ssd < space_ssd; i_ssd++) cout << ' ';
    cout << root_ssd->marks_ssd << "(" << root_ssd->prn_ssd << ")\n";

    printTree_ssd(root_ssd->left_ssd, space_ssd, indent_ssd);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    Student_ssd* root_ssd = nullptr;

    while (true) {
        cout << "\n=== Roll Assignment Menu (ssd) ===\n";
        cout << "1. Insert Student\n2. Assign Rolls & Display\n3. Show Tree (debug)\n4. Exit\nChoice: ";

        int choice_ssd;
        if (!(cin >> choice_ssd)) return 0;

        if (choice_ssd == 1) {
            string prn_ssd, name_ssd;
            int marks_ssd;

            cout << "Enter PRN (no spaces): ";
            cin >> prn_ssd;
            cout << "Enter Name (single token): ";
            cin >> name_ssd;
            cout << "Enter Marks: ";
            cin >> marks_ssd;

            Student_ssd* node_ssd = new Student_ssd(prn_ssd, name_ssd, marks_ssd);
            root_ssd = insert_ssd(root_ssd, node_ssd);
            cout << "Inserted: " << prn_ssd << " " << name_ssd << " " << marks_ssd << "\n";

        } else if (choice_ssd == 2) {
            int counter_ssd = 1;
            assignRollsReverseInorder_ssd(root_ssd, counter_ssd);
            cout << "\nAssigned Rolls (1 = topper):\n";
            displayWithRolls_ssd(root_ssd);

        } else if (choice_ssd == 3) {
            cout << "\nTree (marks(PRN)) - sideways view:\n";
            printTree_ssd(root_ssd);

        } else if (choice_ssd == 4) {
            cout << "Exiting...\n";
            break;

        } else {
            cout << "Invalid choice! Try again.\n";
        }
    }
    return 0;
}
```



## 🧮 Example Execution

Insert: PRN=PRN101 Name=Alice   Marks=85  
Insert: PRN=PRN102 Name=Bob     Marks=92  
Insert: PRN=PRN103 Name=Charlie Marks=85  
Then choose: "Assign Rolls & Display"

Sample Output:
Roll: 1 | PRN: PRN102 | Name: Bob     | Marks: 92
Roll: 2 | PRN: PRN101 | Name: Alice   | Marks: 85
Roll: 3 | PRN: PRN103 | Name: Charlie | Marks: 85


## 🧠 Memory Visualization

            92 (PRN102)
           /
      85 (PRN101)
            \
            85 (PRN103)

Reverse Inorder (Right → Node → Left) visit order:
1. 92 (PRN102)   → Roll 1  
2. 85 (PRN101)   → Roll 2  
3. 85 (PRN103)   → Roll 3  

Thus rolls are correctly assigned: **topper gets 1**.


## ✅ Conclusion

This assignment shows how a **Binary Search Tree** can be used for **ranking and roll number assignment** based on students’ marks.  
Key points learned:
- Inserting records in a BST using **marks** (and **PRN** for ties).  
- Using **reverse inorder traversal** to naturally process students from **highest marks to lowest**.  
- Assigning **roll numbers** in rank order without extra sorting.  
- Visualizing the tree structure to understand how ordering in BST supports ranking.

This strengthens understanding of:
- BST insertion logic with **compound keys** (marks + PRN)  
- Traversal order impact on **ranking logic**  
- Applying data structures to **real-world academic problems** like roll number allocation.
