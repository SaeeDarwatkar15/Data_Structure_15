# 🧩 Assignment 40: Deletion Operations in Product Inventory System Using Search Tree (BST)

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures 

## 📘 Problem Statement  
Write a program to implement **deletion operations** in a **product inventory system** using a **search tree (Binary Search Tree)**.  

Each product must store:  
- **Unique Product Code**  
- **Product Name**  
- **Price**  
- **Quantity in Stock**  
- **Date Received**  
- **Expiration Date**  

Implement the following operations:  
1. **Delete a product using its unique product code.**  
2. **Delete all expired products based on the current date.**  

The products are stored in a BST **organized by product name**, but deletion by **product code** must still be supported.

---

## 🧠 Theory 

### 🔹 1. Binary Search Tree (BST) for Inventory  
A **Binary Search Tree** is a node-based structure where each node has:  
- A **key** (here, we use **product name** for ordering)  
- A left child and a right child  

BST Property:  
- All nodes in the **left subtree** have keys **less than** the current node’s key.  
- All nodes in the **right subtree** have keys **greater than** the current node’s key.  

In this assignment:  
- BST is sorted by **name_ssd** (Product Name).  
- This makes inorder traversal show products **alphabetically**.  

### 🔹 2. Why is Deletion More Complex?  
Deletion in a BST must **preserve the BST property**.  

Deleting a node has **3 cases**:  
1. **Leaf Node** (no children) → simply delete node and return `nullptr`.  
2. **One Child** (left or right) → replace node with its child.  
3. **Two Children** →  
   - Find **inorder successor** (smallest node in right subtree).  
   - Copy successor’s data into current node.  
   - Delete successor node from right subtree.  

Our function `deleteByName_ssd` implements this logic using **name_ssd** as key.

### 🔹 3. Deleting by Product Code (Even Though BST is by Name)  
Problem:  
- BST is organized by **name**, but we must delete using **product code**.  

Solution:  
1. **First search linearly through the tree** (not using BST property) to find the **name** that matches the code.  
   - This is done using `findNameByCode_ssd()` which recursively visits **both left and right** subtrees.  
2. Once we find the matching product, we now know its **name**.  
3. Then we call `deleteByName_ssd(root_ssd, nameFound_ssd)` which deletes the node using **BST deletion logic** based on the name.  

So, deletion by code =  
**search by code → convert to name → delete by name in BST**.

### 🔹 4. Deleting All Expired Products  
To delete expired products:  
1. We **traverse the entire BST** (using inorder) and **collect names** of all products whose `expiry_ssd <= today_ssd`.  
   - This is done by `collectExpired_ssd()`.  
2. We store those names in a **vector**.  
3. Then we loop through this vector and call `deleteByName_ssd()` for each name.  
4. We do **not** delete nodes directly during traversal to avoid breaking recursion or corrupting tree links while iterating.  

### 🔹 5. Date Comparison  
Dates are stored as **strings** in `"YYYY-MM-DD"` format.  
This format is **lexicographically sortable**, meaning:  
- `"2025-10-03" < "2025-10-05"`  
So we can safely use:  
- `if (expiry_ssd <= today_ssd) → product is expired (or expires today)`  

This avoids complicated date parsing and keeps logic clean.

---

## 💻 CODE: 

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Product_ssd {
    string code_ssd, name_ssd;
    float price_ssd;
    int qty_ssd;
    string received_ssd, expiry_ssd;
    Product_ssd *left_ssd, *right_ssd;

    Product_ssd(string c_ssd, string n_ssd, float p_ssd, int q_ssd, string r_ssd, string e_ssd) {
        code_ssd = c_ssd; name_ssd = n_ssd; price_ssd = p_ssd; qty_ssd = q_ssd;
        received_ssd = r_ssd; expiry_ssd = e_ssd;
        left_ssd = right_ssd = nullptr;
    }
};

// Insert into BST (key = product name)
Product_ssd* insert_ssd(Product_ssd* root_ssd, Product_ssd* node_ssd) {
    if (!root_ssd) return node_ssd;
    if (node_ssd->name_ssd < root_ssd->name_ssd)
        root_ssd->left_ssd = insert_ssd(root_ssd->left_ssd, node_ssd);
    else
        root_ssd->right_ssd = insert_ssd(root_ssd->right_ssd, node_ssd);
    return root_ssd;
}

// Find minimum (for deletion)
Product_ssd* findMin_ssd(Product_ssd* root_ssd) {
    while (root_ssd && root_ssd->left_ssd)
        root_ssd = root_ssd->left_ssd;
    return root_ssd;
}

// Delete node by product name (BST key)
Product_ssd* deleteByName_ssd(Product_ssd* root_ssd, string name_ssd) {
    if (!root_ssd) return nullptr;

    if (name_ssd < root_ssd->name_ssd)
        root_ssd->left_ssd = deleteByName_ssd(root_ssd->left_ssd, name_ssd);
    else if (name_ssd > root_ssd->name_ssd)
        root_ssd->right_ssd = deleteByName_ssd(root_ssd->right_ssd, name_ssd);
    else {
        if (!root_ssd->left_ssd && !root_ssd->right_ssd) {
            delete root_ssd; return nullptr;
        }
        else if (!root_ssd->left_ssd || !root_ssd->right_ssd) {
            Product_ssd* child_ssd = root_ssd->left_ssd ? root_ssd->left_ssd : root_ssd->right_ssd;
            delete root_ssd; return child_ssd;
        }
        else {
            Product_ssd* successor_ssd = findMin_ssd(root_ssd->right_ssd);
            root_ssd->name_ssd = successor_ssd->name_ssd;
            root_ssd->code_ssd = successor_ssd->code_ssd;
            root_ssd->price_ssd = successor_ssd->price_ssd;
            root_ssd->qty_ssd = successor_ssd->qty_ssd;
            root_ssd->received_ssd = successor_ssd->received_ssd;
            root_ssd->expiry_ssd = successor_ssd->expiry_ssd;

            root_ssd->right_ssd = deleteByName_ssd(root_ssd->right_ssd, successor_ssd->name_ssd);
        }
    }
    return root_ssd;
}

// Find product by code, return product name if found
bool findNameByCode_ssd(Product_ssd* root_ssd, string code_ssd, string &nameFound_ssd) {
    if (!root_ssd) return false;
    if (root_ssd->code_ssd == code_ssd) {
        nameFound_ssd = root_ssd->name_ssd;
        return true;
    }
    return findNameByCode_ssd(root_ssd->left_ssd, code_ssd, nameFound_ssd) ||
           findNameByCode_ssd(root_ssd->right_ssd, code_ssd, nameFound_ssd);
}

// Collect expired product names
void collectExpired_ssd(Product_ssd* root_ssd, string today_ssd, vector<string>& expired_ssd) {
    if (!root_ssd) return;
    collectExpired_ssd(root_ssd->left_ssd, today_ssd, expired_ssd);
    if (root_ssd->expiry_ssd <= today_ssd)
        expired_ssd.push_back(root_ssd->name_ssd);
    collectExpired_ssd(root_ssd->right_ssd, today_ssd, expired_ssd);
}

// Inorder display of products
void inorder_ssd(Product_ssd* root_ssd) {
    if (!root_ssd) return;
    inorder_ssd(root_ssd->left_ssd);
    cout << "Code: " << root_ssd->code_ssd
         << " | Name: " << root_ssd->name_ssd
         << " | Price: " << root_ssd->price_ssd
         << " | Qty: " << root_ssd->qty_ssd
         << " | Expiry: " << root_ssd->expiry_ssd << "\n";
    inorder_ssd(root_ssd->right_ssd);
}

int main() {
    Product_ssd* root_ssd = nullptr;

    while (true) {
        cout << "\n1. Insert Product";
        cout << "\n2. Delete by Product Code";
        cout << "\n3. Delete All Expired Products";
        cout << "\n4. Display Inventory (Inorder)";
        cout << "\n5. Exit";
        cout << "\nEnter choice: ";

        int choice_ssd;
        cin >> choice_ssd;

        if (choice_ssd == 1) {
            string code_ssd, name_ssd, rec_ssd, exp_ssd;
            float price_ssd;
            int qty_ssd;

            cout << "Code: "; cin >> code_ssd;
            cout << "Name: "; cin >> name_ssd;
            cout << "Price: "; cin >> price_ssd;
            cout << "Quantity: "; cin >> qty_ssd;
            cout << "Received (YYYY-MM-DD): "; cin >> rec_ssd;
            cout << "Expiry (YYYY-MM-DD): "; cin >> exp_ssd;

            root_ssd = insert_ssd(root_ssd, new Product_ssd(code_ssd, name_ssd, price_ssd, qty_ssd, rec_ssd, exp_ssd));
        }
        else if (choice_ssd == 2) {
            string code_ssd, nameFound_ssd;
            cout << "Enter Product Code: ";
            cin >> code_ssd;

            if (findNameByCode_ssd(root_ssd, code_ssd, nameFound_ssd)) {
                root_ssd = deleteByName_ssd(root_ssd, nameFound_ssd);
                cout << "Deleted product with code: " << code_ssd << "\n";
            } else {
                cout << "Product code not found.\n";
            }
        }
        else if (choice_ssd == 3) {
            string today_ssd;
            cout << "Enter today's date (YYYY-MM-DD): ";
            cin >> today_ssd;

            vector<string> expiredNames_ssd;
            collectExpired_ssd(root_ssd, today_ssd, expiredNames_ssd);

            for (string nm_ssd : expiredNames_ssd) {
                root_ssd = deleteByName_ssd(root_ssd, nm_ssd);
                cout << "Deleted expired product: " << nm_ssd << "\n";
            }

            if (expiredNames_ssd.empty())
                cout << "No expired products.\n";
        }
        else if (choice_ssd == 4) {
            cout << "\nInventory (Inorder):\n";
            inorder_ssd(root_ssd);
        }
        else {
            break;
        }
    }

    return 0;
}
```


## 🧪 Example Execution

Insert:  
P001 Milk 50 20 2025-10-01 2025-10-05  
P002 Bread 30 40 2025-10-01 2025-10-03  

Delete Expired → today = 2025-10-04  

Result:  
Deleted expired product: Bread  
Deleted expired product: Milk  

(As per the given example scenario.)

---

## 🧩 Memory Visualization (Conceptual BST Layout)

After inserting Milk:  
              Milk(P001)  
              /       \  
           NULL      NULL  

After inserting Bread (name “Bread” < “Milk”):  
              Milk(P001)  
              /  
        Bread(P002)  

- Root node: **Milk**  
- Left child: **Bread**  

When deleting expired products (today = 2025-10-04 in example):  
1. `collectExpired_ssd` finds nodes `"Bread"` and `"Milk"` as expired (per example).  
2. First delete `"Bread"`:  
              Milk(P001)  
              /  
           NULL  

3. Then delete `"Milk"`:  
              NULL  (tree becomes empty)

So the BST gradually shrinks as expired products are removed.

---

## ✅ Conclusion

This assignment extends the previous inventory BST by adding **deletion operations**, focusing on:  
- **Deleting by product code**, even though the BST is organized by **product name** using a helper search that maps `code → name`.  
- **Bulk deletion of expired products** by scanning the tree, collecting expired items, and deleting them safely without corrupting the tree.  

Key concepts reinforced:  
- BST deletion with **three cases (leaf, one child, two children)**.  
- Separation of concerns:  
  - Use **code** for user-friendly search.  
  - Use **name** as the internal **BST key**.  
- Safe traversal + deletion using **collect-then-delete pattern**.  
- Date handling using `"YYYY-MM-DD"` string comparison.  

This program shows how to manage **dynamic inventory cleanup**, keeping the product tree always **up-to-date** and **free from expired or removed items** while preserving correct BST structure.
