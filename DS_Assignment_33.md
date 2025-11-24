# 🧩 Assignment 33: **BST Creation and Finding Minimum/Maximum**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**
This assignment implements a Binary Search Tree (BST) that supports inserting nodes, finding the minimum and maximum values, and displaying the tree using inorder traversal.

## 📝 Problem Statement
Write a C++ program to:
- Create a BST  
- Insert multiple values  
- Find the **minimum** value  
- Find the **maximum** value  
- Display the BST using **inorder traversal**  
The program should follow BST rules and use dynamic memory allocation.

## 📘 Theory
A **Binary Search Tree (BST)** arranges nodes so that:
- Left subtree contains values **less than** the root  
- Right subtree contains values **greater than** the root  
- No duplicates (ignored)

### Minimum value  
→ Found by moving to the **leftmost node**.

### Maximum value  
→ Found by moving to the **rightmost node**.

### Inorder Traversal  
Left → Root → Right → Produces elements in **ascending sorted order**.

## 💻 CODE 

```cpp
#include <iostream>
using namespace std;

struct Node_ssd {
    int data_ssd;
    Node_ssd *left_ssd;
    Node_ssd *right_ssd;
    Node_ssd(int val_ssd) {
        data_ssd = val_ssd;
        left_ssd = right_ssd = NULL;
    }
};

Node_ssd* insertNode_ssd(Node_ssd *root_ssd, int key_ssd) {
    if (root_ssd == NULL)
        return new Node_ssd(key_ssd);
    if (key_ssd < root_ssd->data_ssd)
        root_ssd->left_ssd = insertNode_ssd(root_ssd->left_ssd, key_ssd);
    else if (key_ssd > root_ssd->data_ssd)
        root_ssd->right_ssd = insertNode_ssd(root_ssd->right_ssd, key_ssd);
    return root_ssd;
}

int findMin_ssd(Node_ssd *root_ssd) {
    if (root_ssd == NULL) { cout << "Tree is empty.\n"; return -1; }
    while (root_ssd->left_ssd != NULL)
        root_ssd = root_ssd->left_ssd;
    return root_ssd->data_ssd;
}

int findMax_ssd(Node_ssd *root_ssd) {
    if (root_ssd == NULL) { cout << "Tree is empty.\n"; return -1; }
    while (root_ssd->right_ssd != NULL)
        root_ssd = root_ssd->right_ssd;
    return root_ssd->data_ssd;
}

void inorder_ssd(Node_ssd *root_ssd) {
    if (root_ssd == NULL) return;
    inorder_ssd(root_ssd->left_ssd);
    cout << root_ssd->data_ssd << " ";
    inorder_ssd(root_ssd->right_ssd);
}

int main() {
    Node_ssd *root_ssd = NULL;
    int choice_ssd, value_ssd;
    while (true) {
        cout << "\n--- BST Operations ---\n";
        cout << "1. Insert\n";
        cout << "2. Find Minimum\n";
        cout << "3. Find Maximum\n";
        cout << "4. Display Inorder\n";
        cout << "5. Exit\n";
        cout << "Enter choice: ";
        cin >> choice_ssd;

        if (choice_ssd == 1) {
            cout << "Enter value to insert: ";
            cin >> value_ssd;
            root_ssd = insertNode_ssd(root_ssd, value_ssd);
        }
        else if (choice_ssd == 2) {
            int min_ssd = findMin_ssd(root_ssd);
            if (min_ssd != -1) cout << "Minimum value: " << min_ssd << endl;
        }
        else if (choice_ssd == 3) {
            int max_ssd = findMax_ssd(root_ssd);
            if (max_ssd != -1) cout << "Maximum value: " << max_ssd << endl;
        }
        else if (choice_ssd == 4) {
            cout << "Inorder Traversal: ";
            inorder_ssd(root_ssd);
            cout << endl;
        }
        else if (choice_ssd == 5) {
            cout << "Exiting.\n";
            break;
        }
        else cout << "Invalid choice.\n";
    }
    return 0;
}
```


## 🧮 Example Execution

--- BST Operations ---
1. Insert
2. Find Minimum
3. Find Maximum
4. Display Inorder
5. Exit
Enter choice: 1
Enter value to insert: 50
Enter choice: 1
Enter value to insert: 30
Enter choice: 1
Enter value to insert: 70
Enter choice: 1
Enter value to insert: 20
Enter choice: 1
Enter value to insert: 40
Enter choice: 1
Enter value to insert: 60
Enter choice: 1
Enter value to insert: 80
Enter choice: 4
Inorder Traversal: 20 30 40 50 60 70 80 
Enter choice: 2
Minimum value: 20
Enter choice: 3
Maximum value: 80
Enter choice: 5
Exiting.


## 🧠 Memory Visualization (BST Structure) 
            50
          /    \
        30      70
       / \     /  \
     20  40  60   80

Inorder (sorted): 20 30 40 50 60 70 80  
Min = 20 (leftmost)  
Max = 80 (rightmost)


## ✅ Conclusion
This assignment reinforces BST insertion and searching concepts. 
Minimum is found by repeatedly going left, while maximum is found by moving right. 
The inorder traversal verifies the sorted nature of BST. 
The program strengthens understanding of recursion, tree structure, sorted data storage, and efficient searching operations.
