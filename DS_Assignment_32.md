# 🧩 Assignment 32: **BST Operations – Count Nodes, Height, Mirror Image**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**
This assignment implements a Binary Search Tree (BST) and performs operations to count nodes, compute the height, generate the mirror image, and display the tree level-wise.

## 📝 Problem Statement
Write a C++ program to:
- Create a Binary Search Tree
- Insert values into the BST
- Count the total number of nodes
- Compute the height of the BST
- Convert the BST into its mirror image
- Display the tree level-wise  

## 📘 Theory
A **Binary Search Tree (BST)** maintains sorted structure:
- Left subtree < Root  
- Right subtree > Root  

### Counting Nodes:
Uses recursion →  
`1 + count(left) + count(right)`

### Height of BST:
Height in levels →  
`1 + max(height(left), height(right))`

### Mirror Image:
Swap left and right subtrees recursively.

### Level Order Display:
Use queue → print node → push children.

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
        if (key_ssd < root_ssd->data_ssd)
            root_ssd->left_ssd = insertNode_ssd(root_ssd->left_ssd, key_ssd);
        else if (key_ssd > root_ssd->data_ssd)
            root_ssd->right_ssd = insertNode_ssd(root_ssd->right_ssd, key_ssd);
        return root_ssd;
    }
    int countNodes_ssd(Node_ssd *root_ssd) {
        if (!root_ssd) return 0;
        return 1 + countNodes_ssd(root_ssd->left_ssd) + countNodes_ssd(root_ssd->right_ssd);
    }
    int height_ssd(Node_ssd *root_ssd) {
        if (!root_ssd) return 0;
        int lh = height_ssd(root_ssd->left_ssd);
        int rh = height_ssd(root_ssd->right_ssd);
        return 1 + (lh > rh ? lh : rh);
    }
    void mirror_ssd(Node_ssd *root_ssd) {
        if (!root_ssd) return;
        Node_ssd *temp = root_ssd->left_ssd;
        root_ssd->left_ssd = root_ssd->right_ssd;
        root_ssd->right_ssd = temp;
        mirror_ssd(root_ssd->left_ssd);
        mirror_ssd(root_ssd->right_ssd);
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
        cout << "\n";
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
            cout << "1. Create tree\n2. Insert\n3. Count nodes\n4. Height\n5. Mirror\n6. Level Display\n7. Exit\n";
            cout << "Enter choice: ";
            cin >> choice_ssd;
            if (choice_ssd == 1) {
                int n; cout << "How many values? "; cin >> n;
                for (int i = 0; i < n; i++) {
                    int v; cout << "Enter value " << i+1 << ": "; cin >> v;
                    root_ssd = insertNode_ssd(root_ssd, v);
                }
            } else if (choice_ssd == 2) {
                int v; cout << "Insert value: "; cin >> v;
                root_ssd = insertNode_ssd(root_ssd, v);
            } else if (choice_ssd == 3) {
                cout << "Total nodes: " << countNodes_ssd(root_ssd) << "\n";
            } else if (choice_ssd == 4) {
                cout << "Height: " << height_ssd(root_ssd) << "\n";
            } else if (choice_ssd == 5) {
                mirror_ssd(root_ssd);
                cout << "Mirror created.\n";
            } else if (choice_ssd == 6) {
                cout << "Level Order: ";
                levelOrder_ssd(root_ssd);
            } else if (choice_ssd == 7) {
                cout << "Exiting.\n";
                break;
            }
        }
        deleteTree_ssd(root_ssd);
        return 0;
    }
```


## 🧮 Example Execution
--- BST Operations ---
1. Create tree
Enter choice: 1  
How many values? 7  
Values: 50 30 70 20 40 60 80  
--- BST Operations ---
Enter choice: 6  
Level order: 50 30 70 20 40 60 80  
Enter choice: 3  
Total nodes: 7  
Enter choice: 4  
Height: 3  
Enter choice: 5  
Mirror created  
Enter choice: 6  
Level order: 50 70 30 80 60 40 20  
Enter choice: 7  
Exiting.


## 🧠 Memory Visualization (Continuous Block)
Initial BST:
            50
          /    \
        30      70
       / \     /  \
     20  40  60   80
Mirror Image:
            50
          /    \
        70      30
       / \     /  \
     80  60  40   20
Node Count = 7  
Height = 3  
Level order (original): 50 30 70 20 40 60 80  
Level order (mirror):   50 70 30 80 60 40 20


## ✅ Conclusion
This assignment demonstrates BST extensions such as counting nodes, computing height, and generating a mirror image. 
These operations strengthen understanding of recursive tree algorithms, subtree processing, and memory organization. 
Level-order traversal verifies the structure at each stage, and the mirror transformation visually shows how left/right pointers are swapped across the entire tree. 
This experiment enhances conceptual clarity of tree transformations, traversal strategies, and recursive design.
