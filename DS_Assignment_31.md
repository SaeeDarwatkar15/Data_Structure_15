# 🧩 Assignment 31: **Binary Search Tree (BST) Operations – Create, Insert, Delete, Levelwise Display**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**

This assignment implements a **Binary Search Tree (BST)** that supports **Create**, **Insert**, **Delete**, and **Level-wise (level-order) Display** using queues and recursive functions.

## 📝 Problem Statement
Write a C++ program to perform the following BST operations:
- Create BST (insert multiple values)
- Insert a new value
- Delete a value
- Display tree level-wise
Use pointer-based node structures and dynamic memory handling.

## 📘 Theory
A **Binary Search Tree** stores data in a sorted hierarchical structure:
- Left subtree < Root  
- Right subtree > Root  
- No duplicates  

### BST Operations:
1. **Insert:** Go left or right based on value until `nullptr` is found.
2. **Delete:**  
   - Node has **0 children** → delete directly  
   - Node has **1 child** → replace node with child  
   - Node has **2 children** → replace with **inorder successor**  
3. **Level Order Traversal:** Use a **queue** to print nodes level-by-level.
4. **Delete Tree:** Use **post-order traversal** to free memory.

## 💻 CODE

```cpp
    #include <iostream>
    #include <queue>
    using namespace std;

    struct Node_ssd {
        int data_ssd;
        Node_ssd *left_ssd;
        Node_ssd *right_ssd;
        Node_ssd(int val_ssd) : data_ssd(val_ssd), left_ssd(nullptr), right_ssd(nullptr) {}
    };

    Node_ssd* insertNode_ssd(Node_ssd *root_ssd, int key_ssd) {
        if (root_ssd == nullptr) return new Node_ssd(key_ssd);
        if (key_ssd < root_ssd->data_ssd) root_ssd->left_ssd = insertNode_ssd(root_ssd->left_ssd, key_ssd);
        else if (key_ssd > root_ssd->data_ssd) root_ssd->right_ssd = insertNode_ssd(root_ssd->right_ssd, key_ssd);
        return root_ssd;
    }

    Node_ssd* findMin_ssd(Node_ssd *root_ssd) {
        while (root_ssd && root_ssd->left_ssd) root_ssd = root_ssd->left_ssd;
        return root_ssd;
    }

    Node_ssd* deleteNode_ssd(Node_ssd *root_ssd, int key_ssd) {
        if (!root_ssd) return nullptr;
        if (key_ssd < root_ssd->data_ssd) root_ssd->left_ssd = deleteNode_ssd(root_ssd->left_ssd, key_ssd);
        else if (key_ssd > root_ssd->data_ssd) root_ssd->right_ssd = deleteNode_ssd(root_ssd->right_ssd, key_ssd);
        else {
            if (!root_ssd->left_ssd) { Node_ssd *temp = root_ssd->right_ssd; delete root_ssd; return temp; }
            else if (!root_ssd->right_ssd) { Node_ssd *temp = root_ssd->left_ssd; delete root_ssd; return temp; }
            Node_ssd *succ = findMin_ssd(root_ssd->right_ssd);
            root_ssd->data_ssd = succ->data_ssd;
            root_ssd->right_ssd = deleteNode_ssd(root_ssd->right_ssd, succ->data_ssd);
        }
        return root_ssd;
    }

    void levelOrder_ssd(Node_ssd *root_ssd) {
        if (!root_ssd) { cout << "Tree is empty.\n"; return; }
        queue<Node_ssd*> q; q.push(root_ssd);
        while (!q.empty()) {
            Node_ssd *curr = q.front(); q.pop();
            cout << curr->data_ssd << " ";
            if (curr->left_ssd) q.push(curr->left_ssd);
            if (curr->right_ssd) q.push(curr->right_ssd);
        }
        cout << endl;
    }

    void deleteTree_ssd(Node_ssd *root_ssd) {
        if (!root_ssd) return;
        deleteTree_ssd(root_ssd->left_ssd);
        deleteTree_ssd(root_ssd->right_ssd);
        delete root_ssd;
    }

    int main() {
        Node_ssd *root_ssd = nullptr;
        int choice_ssd;

        while (true) {
            cout << "\n--- BST Operations ---\n";
            cout << "1. Create tree\n2. Insert\n3. Delete\n4. Level display\n5. Exit\n";
            cout << "Enter choice: ";
            cin >> choice_ssd;

            if (choice_ssd == 1) {
                int n; cout << "How many values? "; cin >> n;
                for (int i = 0; i < n; i++) {
                    int v; cout << "Enter value " << i+1 << ": "; cin >> v;
                    root_ssd = insertNode_ssd(root_ssd, v);
                }
            } 
            else if (choice_ssd == 2) {
                int v; cout << "Insert value: "; cin >> v;
                root_ssd = insertNode_ssd(root_ssd, v);
            } 
            else if (choice_ssd == 3) {
                int v; cout << "Delete value: "; cin >> v;
                root_ssd = deleteNode_ssd(root_ssd, v);
            } 
            else if (choice_ssd == 4) {
                cout << "Level order: "; levelOrder_ssd(root_ssd);
            } 
            else if (choice_ssd == 5) {
                cout << "Exiting...\n";
                break;
            } 
            else cout << "Invalid choice!\n";
        }

        deleteTree_ssd(root_ssd);
        return 0;
    }
```


## 🧮 Example Execution
Create BST → Insert values: 50, 30, 70, 20, 40, 60, 80  
Level order: **50 30 70 20 40 60 80**  
Delete 70  
Level order: **50 30 80 20 40 60**  
Insert 35  
Level order: **50 30 80 20 40 60 35**  
Exit.

## 🧠 Memory Visualization (All inside same block)
Initial insertion of: 50, 30, 70, 20, 40, 60, 80  
        50
      /    \
    30      70
   / \     /  \
 20  40  60   80

After deleting **70** (inorder successor = 80):
        50
      /    \
    30      80
   / \     /
 20  40  60

After inserting **35**:
        50
      /    \
    30      80
   / \     /
 20  40  60
     /
   35

Level-wise traversal prints nodes exactly in BFS order:
- Before deletion: **50 30 70 20 40 60 80**
- After deletion: **50 30 80 20 40 60**
- After inserting 35: **50 30 80 20 40 60 35**


## ✅ Conclusion
This program successfully demonstrates all major **BST operations** including creation, insertion, deletion (handling all 3 cases), 
and level-wise display. Using recursion and queues shows how trees work internally with pointers and dynamic memory. 
The memory visualization clearly shows how the tree structure changes after each operation, helping understand the hierarchical nature of BSTs. 
This assignment improves understanding of **tree traversal, node linking, recursion, searching logic, and real-time memory manipulation**.
