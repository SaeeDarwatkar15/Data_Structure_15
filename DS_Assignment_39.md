# 🧩 Assignment 39: Product Inventory Management System Using Binary Search Tree (BST)

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

## 📘 Problem Statement  
Write a program to implement a **Product Inventory Management System** using a **Binary Search Tree (BST)**.  
Each product must store:  
- **Unique Product Code**  
- **Product Name**  
- **Price**  
- **Quantity in Stock**  
- **Date Received (YYYY-MM-DD)**  
- **Expiration Date (YYYY-MM-DD)**  

Implement the following operations:  
1. Insert a product into the BST (organized by **product name**)  
2. Display all inventory items in **inorder traversal**  
3. List **expired items** using **preorder traversal**  

---

## 🧠 Theory 

A **Binary Search Tree (BST)** is a hierarchical data structure ideal for datasets requiring **quick search, insertion, ordering, and reporting**. In a BST:  
- The **left child** contains data **alphabetically or numerically smaller** than the parent.  
- The **right child** contains data **greater** than the parent.  
- The structure allows operations in **O(log n)** time on average, making it faster than arrays or linked lists.

### ⭐ Why Use BST for Inventory?  
In a shop inventory, we frequently need to:  
- Insert new products  
- Display items in **sorted order**  
- Search quickly  
- Identify expired products  
- Maintain large dynamic data  

A BST organizes data by **product name**, making:  
- Inorder traversal → Alphabetical listing  
- Preorder traversal → Fast root-first processing for expired items  
- Searching for specific products → Much faster  

### ⭐ How Expiry Checking Works  
Dates are stored in **YYYY-MM-DD** format.  
Such strings follow **lexicographic order**, meaning:  
- `"2025-01-02"` < `"2025-02-01"`  
- So direct string comparison is correct and safe.

### ⭐ Traversal Methods Used  
#### ✔ Inorder Traversal (L → Root → R)  
- Gives products **sorted alphabetically** by name.  
- Perfect for displaying full inventory.  

#### ✔ Preorder Traversal (Root → L → R)  
- Used to check **expired products** starting from highest-level items.  
- Faster for generating expiry-based reports.

---

## 💻 CODE 

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Product_ssd {
    string code_ssd;
    string name_ssd;
    float price_ssd;
    int qty_ssd;
    string received_ssd;
    string expiry_ssd;
    Product_ssd *left_ssd, *right_ssd;

    Product_ssd(string c_ssd, string n_ssd, float p_ssd, int q_ssd, string r_ssd, string e_ssd) {
        code_ssd = c_ssd;
        name_ssd = n_ssd;
        price_ssd = p_ssd;
        qty_ssd = q_ssd;
        received_ssd = r_ssd;
        expiry_ssd = e_ssd;
        left_ssd = right_ssd = nullptr;
    }
};

Product_ssd* insert_ssd(Product_ssd* root_ssd, Product_ssd* node_ssd) {
    if (!root_ssd) return node_ssd;
    if (node_ssd->name_ssd < root_ssd->name_ssd)
        root_ssd->left_ssd = insert_ssd(root_ssd->left_ssd, node_ssd);
    else
        root_ssd->right_ssd = insert_ssd(root_ssd->right_ssd, node_ssd);
    return root_ssd;
}

void inorder_ssd(Product_ssd* root_ssd) {
    if (!root_ssd) return;
    inorder_ssd(root_ssd->left_ssd);
    cout << "Code: " << root_ssd->code_ssd << " | Name: " << root_ssd->name_ssd
         << " | Price: " << root_ssd->price_ssd << " | Qty: " << root_ssd->qty_ssd
         << " | Received: " << root_ssd->received_ssd << " | Expiry: " << root_ssd->expiry_ssd << "\n";
    inorder_ssd(root_ssd->right_ssd);
}

void listExpiredPreorder_ssd(Product_ssd* root_ssd, const string &today_ssd) {
    if (!root_ssd) return;
    if (root_ssd->expiry_ssd <= today_ssd)
        cout << "Expired -> Code: " << root_ssd->code_ssd
             << " | Name: " << root_ssd->name_ssd
             << " | Expiry: " << root_ssd->expiry_ssd << "\n";
    listExpiredPreorder_ssd(root_ssd->left_ssd, today_ssd);
    listExpiredPreorder_ssd(root_ssd->right_ssd, today_ssd);
}

int main() {
    Product_ssd* root_ssd = nullptr;
    while (true) {
        cout << "\n1.Insert Product 2.Show All (Inorder) 3.List Expired (Preorder) 4.Exit\nChoice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            string code_ssd, name_ssd, rec_ssd, exp_ssd;
            float price_ssd;
            int qty_ssd;

            cout << "Code: ";
            cin >> code_ssd;
            cout << "Name: ";
            cin >> name_ssd;
            cout << "Price: ";
            cin >> price_ssd;
            cout << "Qty: ";
            cin >> qty_ssd;
            cout << "Received: ";
            cin >> rec_ssd;
            cout << "Expiry: ";
            cin >> exp_ssd;

            root_ssd = insert_ssd(root_ssd, new Product_ssd(code_ssd, name_ssd, price_ssd, qty_ssd, rec_ssd, exp_ssd));

        } else if (ch_ssd == 2) {
            cout << "Inventory (Inorder):\n";
            inorder_ssd(root_ssd);

        } else if (ch_ssd == 3) {
            string today_ssd;
            cout << "Enter today's date: ";
            cin >> today_ssd;
            cout << "Expired items:\n";
            listExpiredPreorder_ssd(root_ssd, today_ssd);

        } else {
            break;
        }
    }
    return 0;
}
```


## 🧪 Example Execution 

Inserted:  
(P001, Milk, 50.0, 20, 2025-10-01, 2025-10-10)  
(P002, Bread, 30.0, 50, 2025-10-05, 2025-10-06)

Inorder Display (Alphabetical Order):  
Bread (P002)  
Milk (P001)

Today = 2025-10-11  
Expired Items:  
Bread (P002)  
Milk (P001)

---

## 🧩 Memory Visualization (BST Structure)

After inserting Milk:  
                     Milk(P001)

After inserting Bread:  
                     Milk(P001)  
                     /  
              Bread(P002)

BST stores nodes in alphabetical order of product name.

---

## ✅ Conclusion

This assignment successfully demonstrates how a **Binary Search Tree** can be used to:  
- Maintain product records  
- Insert and manage inventory efficiently  
- Display items alphabetically using inorder traversal  
- Detect expired products quickly using preorder traversal  
- Store product details in a structured and scalable manner  

The BST ensures **fast search, sorted display, and minimal time complexity**, making it ideal for real-world inventory systems used in shops, supermarkets, and warehouses.

