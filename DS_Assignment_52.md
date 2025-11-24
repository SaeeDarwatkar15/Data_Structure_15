# 🧩 **Assignment 52: Hash Table Implementation using Separate Chaining**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements a **Hash Table with Separate Chaining**, where each index of the hash table stores a **linked list (chain)** of keys.  
This method efficiently handles collisions by allowing multiple keys to be stored in the same bucket.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Implement a **Hash Table** using **Separate Chaining**  
- Perform the operations:
  - **Insert**
  - **Search**
  - **Delete**
  - **Display**
- Accept user-defined table size  
- Handle collisions using **linked lists at each index**

---

## 📘 **Theory**

### 🔢 1. **Hash Table**

A hash table stores data in **buckets** determined by a **hash function**:

```
index = key % table_size
```

---

### 🧩 2. **Collision Resolution — Separate Chaining**

When two keys hash to the **same index**, we store them in a **linked list** (chain) at that index.

Example:

```
Index 0: 10 -> 20 -> 30 -> NULL
```

### ✅ Advantages

- No clustering  
- Table never becomes “full”  
- Easy deletion  

### ❌ Disadvantages

- Might require extra memory  
- Worst-case search becomes **O(n)** if all keys go to same chain  

---

### ⏱️ Time Complexity

| Operation | Average | Worst |
|----------|----------|--------|
| Insert   | O(1)     | O(n)  |
| Search   | O(1)     | O(n)  |
| Delete   | O(1)     | O(n)  |

---

## 💻 **FULL CODE**

```cpp

#include <bits/stdc++.h>
using namespace std;

struct ChainHash_ssd {
    int size_ssd;
    vector<list<int>> table_ssd;

    ChainHash_ssd(int n_ssd) {
        size_ssd = n_ssd;
        table_ssd.resize(size_ssd);
    }

    int hash_ssd(int key_ssd) {
        return key_ssd % size_ssd;
    }

    void insert_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        for (int x_ssd : table_ssd[idx_ssd]) {
            if (x_ssd == key_ssd) return; // duplicate key found
        }
        table_ssd[idx_ssd].push_back(key_ssd);
    }

    bool search_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        for (int x_ssd : table_ssd[idx_ssd]) {
            if (x_ssd == key_ssd) 
                return true;
        }
        return false;
    }

    bool delete_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        for (auto it = table_ssd[idx_ssd].begin(); it != table_ssd[idx_ssd].end(); ++it) {
            if (*it == key_ssd) {
                table_ssd[idx_ssd].erase(it);
                return true;
            }
        }
        return false;
    }

    void display_ssd() {
        cout << "\nHash Table (Separate Chaining):\n";
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            cout << i_ssd << " : ";
            for (int x_ssd : table_ssd[i_ssd]) {
                cout << x_ssd << " -> ";
            }
            cout << "NULL\n";
        }
    }
};

int main() {
    int n_ssd;
    cout << "Enter hash table size: ";
    cin >> n_ssd;

    ChainHash_ssd ht_ssd(n_ssd);

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
            cout << "Inserted.\n";
        }
        else if (ch_ssd == 2) {
            int key_ssd;
            cout << "Enter key: ";
            cin >> key_ssd;
            cout << (ht_ssd.search_ssd(key_ssd) ? "Found\n" : "Not Found\n");
        }
        else if (ch_ssd == 3) {
            int key_ssd;
            cout << "Enter key: ";
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

    return 0;
}
```

---

## 🧪 **Example Execution**

**Input:**

```text
table size: 5
insert keys: 10, 15, 20, 7
search: 20
delete: 15
display
```

**Output:**

```text
0 : 10 -> 15 -> 20 -> NULL
1 : 7 -> NULL
2 : NULL
3 : NULL
4 : NULL

Search result: Found

After deletion:
0 : 10 -> 20 -> NULL
```

---

## ✅ **Conclusion**

In this assignment, we successfully implemented **Separate Chaining Hashing** using:

- **Linked lists** at each bucket  
- A modulo-based **hash function**  
- Operations for **Insert**, **Search**, **Delete**, **Display**

This helps understand:

- How collisions are resolved in hashing  
- The internal working of chained hash tables  
- Use of dynamic structures like **linked lists**  
- Efficient storage and lookup using hashing  

This method ensures that collision handling is smooth and efficient even when many keys hash to the same index.
