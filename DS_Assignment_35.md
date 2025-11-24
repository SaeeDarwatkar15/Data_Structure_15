# 🧩 Assignment 35: **Recursive Binary Tree Operations (Inorder, Preorder, Leaf Count, Mirror)**  

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  
This assignment creates a **Binary Tree** and performs the following **recursive operations**:  
- Inorder Traversal  
- Preorder Traversal  
- Display Number of Leaf Nodes  
- Mirror Image of the tree  

## 📝 Problem Statement  
Write a C++ program to:  
1. **Create a Binary Tree** interactively (using `-1` for NULL child).  
2. Perform **recursive**:  
   - Inorder Traversal  
   - Preorder Traversal  
   - Leaf Node Count  
   - Mirror Image transformation  
3. Provide a **menu-driven interface** so the user can choose the operation.

## 📘 Theory  

### Binary Tree  
A **Binary Tree** is a node-based structure where each node has at most two children:  
- `left_ssd` child  
- `right_ssd` child  

There is **no ordering rule** like BST here; it is a general binary tree.

### Recursive Inorder Traversal (LNR)  
Inorder = **Left → Node → Right**  
Recursive logic:  
- Recur on left subtree  
- Visit (print) root  
- Recur on right subtree  

### Recursive Preorder Traversal (NLR)  
Preorder = **Node → Left → Right**  
Recursive logic:  
- Visit (print) root  
- Recur on left subtree  
- Recur on right subtree  

### Leaf Node Count  
A **leaf node** is a node with:  
- `left_ssd == NULL` and `right_ssd == NULL`  
Recursive counting:  
- If node is NULL → 0  
- If node is leaf → 1  
- Else → count(left) + count(right)  

### Mirror Image  
To create mirror image of a tree, for every node:  
- Swap `left_ssd` and `right_ssd`  
- Recursively mirror left and right children  
This flips the tree horizontally.

## 💻 CODE 

```cpp
#include <iostream>
using namespace std;

struct Node_ssd {
    int data_ssd;
    Node_ssd *left_ssd, *right_ssd;

    Node_ssd(int val_ssd) {
        data_ssd = val_ssd;
        left_ssd = right_ssd = NULL;
    }
};

Node_ssd* create_ssd() {
    int val_ssd;
    cout << "Enter value (-1 for NULL): ";
    cin >> val_ssd;

    if (val_ssd == -1) return NULL;

    Node_ssd* node = new Node_ssd(val_ssd);
    cout << "Left of " << val_ssd << ": ";
    node->left_ssd = create_ssd();
    cout << "Right of " << val_ssd << ": ";
    node->right_ssd = create_ssd();

    return node;
}

void inorder_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return;
    inorder_ssd(root_ssd->left_ssd);
    cout << root_ssd->data_ssd << " ";
    inorder_ssd(root_ssd->right_ssd);
}

void preorder_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return;
    cout << root_ssd->data_ssd << " ";
    preorder_ssd(root_ssd->left_ssd);
    preorder_ssd(root_ssd->right_ssd);
}

int leafCount_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return 0;
    if (!root_ssd->left_ssd && !root_ssd->right_ssd) return 1;
    return leafCount_ssd(root_ssd->left_ssd) + leafCount_ssd(root_ssd->right_ssd);
}

void mirror_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return;
    swap(root_ssd->left_ssd, root_ssd->right_ssd);
    mirror_ssd(root_ssd->left_ssd);
    mirror_ssd(root_ssd->right_ssd);
}

int main() {
    cout << "Create Binary Tree:\n";
    Node_ssd* root_ssd = create_ssd();

    int choice_ssd;

    while (true) {
        cout << "\n======= MENU =======\n";
        cout << "1. Recursive Inorder\n";
        cout << "2. Recursive Preorder\n";
        cout << "3. Leaf Count\n";
        cout << "4. Mirror Tree\n";
        cout << "5. Exit\n";
        cout << "Enter choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1:
                cout << "Inorder: ";
                inorder_ssd(root_ssd);
                cout << "\n";
                break;

            case 2:
                cout << "Preorder: ";
                preorder_ssd(root_ssd);
                cout << "\n";
                break;

            case 3:
                cout << "Leaf Nodes = " << leafCount_ssd(root_ssd) << "\n";
                break;

            case 4:
                mirror_ssd(root_ssd);
                cout << "Mirror created!\n";
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

Example Tree :
    1
   / \
  2   3

Example Output for this tree:
Inorder  → 2 1 3  
Preorder → 1 2 3  
Leaf Count → 2  

After Mirror:
Original:
    1
   / \
  2   3

Mirror:
    1
   / \
  3   2  

New traversals after mirror:
Inorder  → 3 1 2  
Preorder → 1 3 2  
Leaf Count → 2  (unchanged)


## 🧠 Memory Visualization

Original links:
root(1)->left = 2, root(1)->right = 3  
Node 2: left = NULL, right = NULL (leaf)  
Node 3: left = NULL, right = NULL (leaf)  

After mirror:
root(1)->left = 3, root(1)->right = 2  
Still two leaves: nodes 2 and 3.


## ✅ Conclusion  
This assignment demonstrates **recursive processing** over a binary tree:
- **Recursive Inorder** and **Preorder** traversals show how simple and natural recursion is for tree traversal.
- **Leaf Count** illustrates how recursive functions can aggregate information from subtrees.
- **Mirror Image** transformation shows how changing child links at each node affects the overall shape of the tree but does **not** change the number of leaf nodes.

It strengthens understanding of:
- Recursive function calls on hierarchical structures  
- Different traversal orders and their outputs  
- Structure transformation of binary trees by swapping children  
Overall, this exercise improves your confidence in working with **recursive algorithms** on tree data structures in C++.
