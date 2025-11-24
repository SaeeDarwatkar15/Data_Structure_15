# 🧩 Assignment 37: **Operations on a Binary Search Tree (BST) with Numeric Keys**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  
This assignment illustrates basic operations on a **Binary Search Tree (BST)** that stores **integer keys**, including **insert**, **delete**, **search**, and **inorder (sorted) display**.

## 📝 Problem Statement
Write a C++ program to:
- Create a **BST** that stores **numeric keys**  
- Perform the following operations:
  1. **Insert** a key into the BST  
  2. **Delete** a key from the BST  
  3. **Search (Find)** a key in the BST  
  4. **Display** all keys in **sorted (inorder)** order  
Use a **menu-driven interface** and implement proper BST logic.

## 📘 Theory (BST Basics)
A **Binary Search Tree (BST)** is a special binary tree where:
- For every node:
  - All keys in the **left subtree** are **less than** the node’s key  
  - All keys in the **right subtree** are **greater than** the node’s key  

### 1️⃣ Insert Operation
To insert a key:
- If tree is empty → new node becomes **root**  
- If key < current node → go to **left child**  
- If key > current node → go to **right child**  
- Recursively find the correct NULL position and create a new node there.

### 2️⃣ Search Operation
To search a key:
- If node is NULL → key **not found**  
- If key == node key → **found**  
- If key < node key → **search in left subtree**  
- If key > node key → **search in right subtree**

### 3️⃣ Delete Operation
Cases when deleting a node:
1. **No child (leaf node)** → delete the node, return NULL  
2. **One child** → replace node with its single child  
3. **Two children** →  
   - Find **inorder successor** (smallest key in right subtree)  
   - Copy its key into current node  
   - Delete that successor node from right subtree  

### 4️⃣ Inorder Traversal (Sorted Display)
Inorder = **Left → Node → Right**  
For a BST, inorder traversal prints keys in **ascending sorted order**.

## 💻 CODE 

```cpp
#include <iostream>
using namespace std;

struct Node_ssd {
    int key_ssd;
    Node_ssd *left_ssd, *right_ssd;

    Node_ssd(int k_ssd) {
        key_ssd = k_ssd;
        left_ssd = right_ssd = nullptr;
    }
};

// INSERT
Node_ssd* insert_ssd(Node_ssd* root_ssd, int key_ssd) {
    if (!root_ssd)
        return new Node_ssd(key_ssd);

    if (key_ssd < root_ssd->key_ssd)
        root_ssd->left_ssd = insert_ssd(root_ssd->left_ssd, key_ssd);
    else if (key_ssd > root_ssd->key_ssd)
        root_ssd->right_ssd = insert_ssd(root_ssd->right_ssd, key_ssd);

    return root_ssd;
}

// FIND MIN (used in deletion)
Node_ssd* findMin_ssd(Node_ssd* root_ssd) {
    while (root_ssd && root_ssd->left_ssd)
        root_ssd = root_ssd->left_ssd;
    return root_ssd;
}

// DELETE
Node_ssd* delete_ssd(Node_ssd* root_ssd, int key_ssd) {
    if (!root_ssd)
        return nullptr;

    if (key_ssd < root_ssd->key_ssd)
        root_ssd->left_ssd = delete_ssd(root_ssd->left_ssd, key_ssd);
    else if (key_ssd > root_ssd->key_ssd)
        root_ssd->right_ssd = delete_ssd(root_ssd->right_ssd, key_ssd);
    else {
        // Node found
        if (!root_ssd->left_ssd && !root_ssd->right_ssd) {
            // Case 1: no child
            delete root_ssd;
            return nullptr;
        }
        else if (!root_ssd->left_ssd || !root_ssd->right_ssd) {
            // Case 2: one child
            Node_ssd* child_ssd = root_ssd->left_ssd ? root_ssd->left_ssd : root_ssd->right_ssd;
            delete root_ssd;
            return child_ssd;
        }
        else {
            // Case 3: two children
            Node_ssd* succ_ssd = findMin_ssd(root_ssd->right_ssd);
            root_ssd->key_ssd = succ_ssd->key_ssd;
            root_ssd->right_ssd = delete_ssd(root_ssd->right_ssd, succ_ssd->key_ssd);
        }
    }
    return root_ssd;
}

// SEARCH
bool find_ssd(Node_ssd* root_ssd, int key_ssd) {
    if (!root_ssd)
        return false;

    if (root_ssd->key_ssd == key_ssd)
        return true;

    if (key_ssd < root_ssd->key_ssd)
        return find_ssd(root_ssd->left_ssd, key_ssd);
    else
        return find_ssd(root_ssd->right_ssd, key_ssd);
}

// INORDER (sorted display)
void inorder_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return;
    inorder_ssd(root_ssd->left_ssd);
    cout << root_ssd->key_ssd << " ";
    inorder_ssd(root_ssd->right_ssd);
}

// MAIN MENU
int main() {
    Node_ssd* root_ssd = nullptr;
    int choice_ssd, value_ssd;

    while (true) {
        cout << "\n=== BST MENU (ssd) ===\n";
        cout << "1. Insert Key\n";
        cout << "2. Delete Key\n";
        cout << "3. Find Key\n";
        cout << "4. Show (Inorder)\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value_ssd;
                root_ssd = insert_ssd(root_ssd, value_ssd);
                break;

            case 2:
                cout << "Enter value to delete: ";
                cin >> value_ssd;
                root_ssd = delete_ssd(root_ssd, value_ssd);
                break;

            case 3:
                cout << "Enter value to find: ";
                cin >> value_ssd;
                cout << (find_ssd(root_ssd, value_ssd) ?
                    "Key FOUND\n" : "Key NOT FOUND\n");
                break;

            case 4:
                cout << "Inorder (sorted): ";
                inorder_ssd(root_ssd);
                cout << "\n";
                break;

            case 5:
                return 0;

            default:
                cout << "Invalid choice!\n";
        }
    }
}
```


## 🧮 Example Execution

Insert: 40
Insert: 20
Insert: 60
Insert: 10
Insert: 30
Show
Output:
Inorder (sorted): 10 20 30 40 60

Find operation:
Find: 30
→ Key FOUND

Delete operation:
Delete: 20
Show
Inorder (sorted): 10 30 40 60


## 🧠 Memory Visualization

        40
       /  \
     20    60
    /  \
  10   30

After deleting 20 (node with two children, replaced by inorder successor 30):
        40
       /  \
     30    60
    /
  10

Inorder traversal reflects this structure exactly:
Before delete: 10 20 30 40 60
After delete:  10 30 40 60


## ✅ Conclusion
This assignment demonstrates all core operations on a **Binary Search Tree with numeric keys**:
- **Insertion** maintains BST ordering property  
- **Search** efficiently locates keys using comparisons and branching  
- **Deletion** correctly handles all three cases (leaf, one child, two children using inorder successor)  
- **Inorder traversal** provides a natural way to **display keys in sorted order**

It strengthens understanding of:
- Recursive algorithms on trees  
- How structural changes (like deletion) affect the shape of the BST  
- Using inorder, min, successor concepts to preserve BST properties  

Overall, this program gives a clear, practical illustration of how **BSTs support dynamic ordered data storage and efficient searching**.
