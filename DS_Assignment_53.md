# 🧩 **Assignment 53: Hash Table Implementation Using Linked Lists (Separate Chaining – Custom Linked List)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment implements **collision resolution using linked lists**, where each hash table index stores a **custom singly linked list**.  
This is another form of **Separate Chaining**, but instead of STL list, we manually implement the nodes.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Implement a **Hash Table** using **Separate Chaining with Linked Lists**  
- Allow the operations:
  - Insert  
  - Search  
  - Delete  
  - Display  
- Accept table size from user  
- Handle collisions by inserting colliding keys into a linked list at that index  

---

## 📘 **Theory**

### 🔢 1. **Hash Table Basics**

Hash table uses a **hash function**:

```
index = key % table_size
```

This gives the position where the key should be inserted.

---

### ⚠️ 2. **Collision Handling Using Linked Lists**

If two keys map to the same index:

- A **linked list** is created at that index  
- New keys are inserted at **head** for constant-time insertion  

Example:

```
Index 2: 22 -> 17 -> 12 -> 7 -> NULL
```

### 📌 Benefits

- Unlimited chaining — never becomes "full" like linear probing  
- Efficient insert/delete/search operations  

### 🧨 Drawbacks

- Requires extra memory for pointers  
- Worst-case search becomes O(n)  

---

### ⏱️ **Time Complexity**

| Operation | Avg Time | Worst Time |
|----------|-----------|-------------|
| Insert   | O(1)      | O(n)        |
| Search   | O(1)      | O(n)        |
| Delete   | O(1)      | O(n)        |

---

## 💻 **FULL C++ PROGRAM**

```cpp

#include <bits/stdc++.h>
using namespace std;

struct Node_ssd {
    int key_ssd;
    Node_ssd* next_ssd;
    Node_ssd(int k_ssd) {
        key_ssd = k_ssd;
        next_ssd = nullptr;
    }
};

struct HashList_ssd {
    int size_ssd;
    vector<Node_ssd*> table_ssd;

    HashList_ssd(int n_ssd) {
        size_ssd = n_ssd;
        table_ssd.assign(size_ssd, nullptr);
    }

    int hash_ssd(int key_ssd) {
        return key_ssd % size_ssd;
    }

    void insert_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);

        // check for duplicates
        Node_ssd* curr = table_ssd[idx_ssd];
        while (curr) {
            if (curr->key_ssd == key_ssd)
                return;
            curr = curr->next_ssd;
        }

        // insert at head
        Node_ssd* node = new Node_ssd(key_ssd);
        node->next_ssd = table_ssd[idx_ssd];
        table_ssd[idx_ssd] = node;
    }

    bool search_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        Node_ssd* curr = table_ssd[idx_ssd];

        while (curr) {
            if (curr->key_ssd == key_ssd)
                return true;
            curr = curr->next_ssd;
        }
        return false;
    }

    bool delete_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        Node_ssd* curr = table_ssd[idx_ssd];
        Node_ssd* prev = nullptr;

        while (curr) {
            if (curr->key_ssd == key_ssd) {
                if (prev)
                    prev->next_ssd = curr->next_ssd;
                else
                    table_ssd[idx_ssd] = curr->next_ssd;

                delete curr;
                return true;
            }
            prev = curr;
            curr = curr->next_ssd;
        }
        return false;
    }

    void display_ssd() {
        cout << "\nHash Table (Linked Lists):\n";
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            cout << i_ssd << " : ";
            Node_ssd* curr = table_ssd[i_ssd];
            while (curr) {
                cout << curr->key_ssd << " -> ";
                curr = curr->next_ssd;
            }
            cout << "NULL\n";
        }
    }
};

int main() {
    int n_ssd;
    cout << "Enter hash table size: ";
    cin >> n_ssd;

    HashList_ssd ht_ssd(n_ssd);

    while (true) {
        cout << "\n1.Insert  2.Search  3.Delete  4.Display  5.Exit\n";
        cout << "Enter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int key_ssd;
            cout << "Enter key: ";
            cin >> key_ssd;
            ht_ssd.insert_ssd(key_ssd);
            cout << "Inserted\n";
        }
        else if (ch_ssd == 2) {
            int key_ssd;
            cout << "Enter key: ";
            cin >> key_ssd;
            cout << (ht_ssd.search_ssd(key_ssd) ? "Found\n" : "Not Found\n");
        }
        else if (ch_ssd == 3) {
            int key_ssd;
            cout << "Enter key to delete: ";
            cin >> key_ssd;
            cout << (ht_ssd.delete_ssd(key_ssd) ? "Deleted\n" : "Not Found\n");
        }
        else if (ch_ssd == 4) {
            ht_ssd.display_ssd();
        }
        else {
            cout << "Exiting...\n";
            break;
        }
    }
}
```

---

## 🧪 **Example Execution**

### **Input**
```
size = 5
insert: 7, 12, 17, 22
display
```

### **Output**
```
0 : NULL
1 : NULL
2 : 22 -> 17 -> 12 -> 7 -> NULL
3 : NULL
4 : NULL
```

---

## ✅ **Conclusion**

In this assignment, we have implemented a **Hash Table using Linked List chaining**, using our own **Node structure** instead of STL.  
We successfully completed:

- Insertion at head  
- Searching through a linked list  
- Deletion with pointer adjustment  
- Displaying the entire table  

This assignment helps you understand:

- How collisions are handled manually using linked lists  
- How hashing distributes keys  
- Pointer-based dynamic memory management  
- Internal working of chained-hashing structures  

This is a strong foundation for understanding **hash maps**, **dictionaries**, and **database indexing techniques**.  
