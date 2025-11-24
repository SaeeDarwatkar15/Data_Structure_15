# 🧩 Assignment 34: **Non-Recursive Binary Tree Operations (Inorder, Preorder, Leaf Count, Mirror)**  

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**

This assignment implements a **Binary Tree** and performs the following **non-recursive** operations:  
- **Inorder Traversal** (using stack)  
- **Preorder Traversal** (using stack)  
- **Count the number of leaf nodes** (recursive)  
- **Mirror Image** of the tree (swap left & right recursively)

## 📝 Problem Statement
Write a C++ program to:  
1. **Create a Binary Tree** interactively (using `-1` for NULL child).  
2. Perform the following operations:  
   - **Inorder Traversal (Non-Recursive)**  
   - **Preorder Traversal (Non-Recursive)**  
   - **Count total leaf nodes**  
   - **Create mirror image** of the tree  
3. Provide a **menu-driven interface** for all operations.

## 📘 Theory

### Binary Tree
A **Binary Tree** is a hierarchical structure where each node has at most **two children**:
- `left_ssd` child  
- `right_ssd` child  

Unlike a BST, here **no ordering rule** is required between left and right.

---

### Non-Recursive Inorder Traversal (LNR)
Inorder = **Left → Node → Right**  
To do it **without recursion**, we use a **stack**:

1. Start at `root`  
2. Go left while pushing nodes on the stack  
3. Pop from stack → visit node  
4. Move to its right child  
5. Repeat until stack empty and current is NULL  

This simulates the **call stack** of recursion manually.

---

### Non-Recursive Preorder Traversal (NLR)
Preorder = **Node → Left → Right**  
Using stack:

1. Push `root`  
2. While stack not empty:  
   - Pop top → visit  
   - Push **right child** (if any)  
   - Push **left child** (if any)  

We push right first so that left is processed first (LIFO behavior).

---

### Leaf Node Count
A **leaf node** has:
- `left_ssd == NULL`  
- `right_ssd == NULL`  

We count recursively:
- If node is NULL → 0  
- If node is leaf → 1  
- Else → count(left) + count(right)

---

### Mirror Image of Tree
To create **mirror image**:
- For every node, **swap** `left_ssd` and `right_ssd`  
- Then recursively mirror its children  

This flips the tree horizontally.

---

## 💻 CODE 

```cpp
#include <iostream>
#include <stack>
using namespace std;

struct Node_ssd {
    int data_ssd;
    Node_ssd *left_ssd, *right_ssd;
    Node_ssd(int val_ssd) {
        data_ssd = val_ssd;
        left_ssd = right_ssd = NULL;
    }
};

// Create Binary Tree (recursive input, -1 for NULL)
Node_ssd* create_ssd() {
    int val_ssd;
    cout << "Enter value (-1 for NULL): ";
    cin >> val_ssd;

    if (val_ssd == -1) return NULL;

    Node_ssd* root_ssd = new Node_ssd(val_ssd);
    cout << "Left of " << val_ssd << ": ";
    root_ssd->left_ssd = create_ssd();
    cout << "Right of " << val_ssd << ": ";
    root_ssd->right_ssd = create_ssd();

    return root_ssd;
}

// Inorder Non-Recursive (LNR)
void inorderNonRec_ssd(Node_ssd* root_ssd) {
    stack<Node_ssd*> st;
    Node_ssd* curr = root_ssd;
    while (curr || !st.empty()) {
        while (curr) {
            st.push(curr);
            curr = curr->left_ssd;
        }
        curr = st.top(); st.pop();
        cout << curr->data_ssd << " ";
        curr = curr->right_ssd;
    }
}

// Preorder Non-Recursive (NLR)
void preorderNonRec_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return;
    stack<Node_ssd*> st;
    st.push(root_ssd);
    while (!st.empty()) {
        Node_ssd* curr = st.top(); st.pop();
        cout << curr->data_ssd << " ";
        if (curr->right_ssd) st.push(curr->right_ssd);
        if (curr->left_ssd) st.push(curr->left_ssd);
    }
}

// Leaf Count (recursive)
int leafCount_ssd(Node_ssd* root_ssd) {
    if (!root_ssd) return 0;
    if (!root_ssd->left_ssd && !root_ssd->right_ssd) return 1;
    return leafCount_ssd(root_ssd->left_ssd) + leafCount_ssd(root_ssd->right_ssd);
}

// Mirror Image
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
        cout << "1. Inorder (Non-Recursive)\n";
        cout << "2. Preorder (Non-Recursive)\n";
        cout << "3. Leaf Count\n";
        cout << "4. Mirror Tree\n";
        cout << "5. Exit\n";
        cout << "Enter choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1:
                cout << "Inorder Traversal: ";
                inorderNonRec_ssd(root_ssd);
                cout << "\n";
                break;
            case 2:
                cout << "Preorder Traversal: ";
                preorderNonRec_ssd(root_ssd);
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
        }
    }
}
```


## 🧮 Example Execution

Example Tree:
    1
   / \
  2   3

 
Inorder (Non-Recursive)   → 2 1 3
Preorder (Non-Recursive)  → 1 2 3
Leaf Count                → 2

If we create mirror of this tree:
Original:
    1
   / \
  2   3
Mirror:
    1
   / \
  3   2

After Mirror:
Inorder (Non-Recursive)   → 3 1 2
Preorder (Non-Recursive)  → 1 3 2
Leaf Count remains        → 2  (mirror does not change number of leaves)


## 🧠 Memory / Tree Visualization

Original Tree in Memory (Example):

        [1]
       /   \
    [2]     [3]

- Node 1: `left_ssd → Node 2`, `right_ssd → Node 3`  
- Node 2: `left_ssd = NULL`, `right_ssd = NULL` (leaf)  
- Node 3: `left_ssd = NULL`, `right_ssd = NULL` (leaf)  

After Mirror:

        [1]
       /   \
    [3]     [2]

Links swapped at each node:
- Node 1: `left_ssd → Node 3`, `right_ssd → Node 2`  

But **leaf nodes** still: Node 2, Node 3 → total = 2.


## ✅ Conclusion
This assignment demonstrates how to perform important **non-recursive operations** on a **Binary Tree**:
- **Inorder and Preorder traversals using a stack** (simulating recursion manually)  
- **Counting leaf nodes recursively**, which strengthens understanding of recursive tree processing  
- **Generating a mirror image** by swapping children at every node, visualizing structural tree transformations  

It helps to:
- Understand how **stacks** can replace recursion in tree traversals  
- See how **leaf nodes** represent terminal points of paths  
- Observe how the **shape of the tree changes** with mirroring, while properties like leaf count remain unchanged  

Overall, this assignment deepens your understanding of **binary tree structure, non-recursive algorithms, recursion vs stack simulation, and tree transformation techniques** in C++.
