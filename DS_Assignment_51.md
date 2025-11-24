# 🧩 **Assignment 51: Hash Table Implementation using Linear Probing**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment implements a **Hash Table (Open Addressing)** with **Linear Probing** for collision resolution.  
The program supports **Insert**, **Search**, **Delete**, and **Display** operations on the hash table.  
It uses simple hashing `key % table_size` and resolves collisions by checking the next available slot.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Implement a **Hash Table** using **Open Addressing (Linear Probing)**  
- Perform operations:
  - **Insert**
  - **Search**
  - **Delete**
  - **Display**
- Accept user-defined table size  
- Handle collision using **linear probing**  
- Mark deleted elements using *lazy deletion* (using a deleted flag)

---

## 📘 **Theory**

### 🔢 1. **Hash Table**

A **Hash Table** is a data structure that stores key–value pairs and provides:

- **O(1)** average time for insert  
- **O(1)** average time for search  
- **O(1)** average time for delete  

It uses a **hash function**:

```
index = key % table_size
```

---

### ⚠️ 2. **Collision Handling**

A collision occurs when two keys hash to the **same index**.

We resolve this using **Linear Probing**:

```
index = (hash + i) % size
for i = 0, 1, 2, ... until a free slot is found
```

If the slot is:

- `<empty>` → insert key  
- `<deleted>` → insert key  
- `occupied` → check next position  

---

### 🗑️ 3. **Deletion Using Lazy Deletion**

Instead of removing the element completely:

- We mark slot as `<deleted>`  
- Allows search to continue for other keys in the probe sequence  

---

### 💡 4. **Advantages of Linear Probing**

- Simple and easy to implement  
- Fast memory access due to contiguous locations  

### ❗ Drawback

- **Primary clustering** — long sequences of filled slots may form  

---

### ⏱️ Time Complexity

| Operation | Average Case | Worst Case |
|----------|--------------|------------|
| Insert   | O(1)         | O(n)       |
| Search   | O(1)         | O(n)       |
| Delete   | O(1)         | O(n)       |

---

## 💻 **FULL CODE**

```cpp
#include <bits/stdc++.h>
using namespace std;

struct HashEntry_ssd {
    int key_ssd;
    bool occupied_ssd;
    bool deleted_ssd;

    HashEntry_ssd() {
        key_ssd = 0;
        occupied_ssd = false;
        deleted_ssd = false;
    }
};

struct HashTable_ssd {
    int size_ssd;
    vector<HashEntry_ssd> table_ssd;

    HashTable_ssd(int n) {
        size_ssd = n;
        table_ssd.resize(size_ssd);
    }

    int hash_ssd(int key_ssd) {
        return key_ssd % size_ssd;
    }

    bool insert_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            int pos_ssd = (idx_ssd + i_ssd) % size_ssd;
            if (!table_ssd[pos_ssd].occupied_ssd || table_ssd[pos_ssd].deleted_ssd) {
                table_ssd[pos_ssd].key_ssd = key_ssd;
                table_ssd[pos_ssd].occupied_ssd = true;
                table_ssd[pos_ssd].deleted_ssd = false;
                return true;
            }
        }
        return false;
    }

    int search_ssd(int key_ssd) {
        int idx_ssd = hash_ssd(key_ssd);
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            int pos_ssd = (idx_ssd + i_ssd) % size_ssd;

            if (!table_ssd[pos_ssd].occupied_ssd && !table_ssd[pos_ssd].deleted_ssd)
                return -1;

            if (table_ssd[pos_ssd].occupied_ssd && table_ssd[pos_ssd].key_ssd == key_ssd)
                return pos_ssd;
        }
        return -1;
    }

    bool delete_ssd(int key_ssd) {
        int pos_ssd = search_ssd(key_ssd);
        if (pos_ssd == -1) return false;
        table_ssd[pos_ssd].occupied_ssd = false;
        table_ssd[pos_ssd].deleted_ssd = true;
        return true;
    }

    void display_ssd() {
        cout << "\nIndex  :  Entry\n";
        for (int i = 0; i < size_ssd; i++) {
            cout << i << " : ";
            if (table_ssd[i].occupied_ssd)
                cout << table_ssd[i].key_ssd;
            else if (table_ssd[i].deleted_ssd)
                cout << "<deleted>";
            else
                cout << "<empty>";
            cout << "\n";
        }
    }
};

int main() {
    int n_ssd;
    cout << "Enter table size: ";
    cin >> n_ssd;

    HashTable_ssd ht_ssd(n_ssd);

    while (true) {
        cout << "\n1.Insert 2.Search 3.Delete 4.Display 5.Exit\nEnter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int k_ssd;
            cout << "Enter key: ";
            cin >> k_ssd;
            if (ht_ssd.insert_ssd(k_ssd))
                cout << "Inserted\n";
            else
                cout << "Failed (table full)\n";
        }
        else if (ch_ssd == 2) {
            int k_ssd;
            cout << "Enter key: ";
            cin >> k_ssd;
            int pos_ssd = ht_ssd.search_ssd(k_ssd);
            if (pos_ssd == -1)
                cout << "Not found\n";
            else
                cout << "Found at index " << pos_ssd << "\n";
        }
        else if (ch_ssd == 3) {
            int k_ssd;
            cout << "Enter key: ";
            cin >> k_ssd;
            cout << (ht_ssd.delete_ssd(k_ssd) ? "Deleted\n" : "Not found\n");
        }
        else if (ch_ssd == 4)
            ht_ssd.display_ssd();
        else
            break;
    }
}
```

---

## 🧮 **Example Execution**

**Input:**

```text
Table size: 7
Insert keys: 10, 17, 24, 31
Search key: 24
Delete key: 17
```

**Output:**

```text
Index  :  Entry
0 : <empty>
1 : 31
2 : 10
3 : <deleted>
4 : 24
5 : <empty>
6 : <empty>

Search result: Found at index 4
Key 17 deleted
```

---

## ✅ **Conclusion**

In this assignment, we implemented a **Hash Table with Linear Probing** that supports:

- Efficient **Insert**, **Search**, **Delete** operations  
- Proper collision resolution using **sequential probing**  
- Safe deletion using **lazy deletion** flags  
- Clear traversal through a **Display** operation  

This assignment strengthens understanding of:

- **Hash functions**  
- **Open addressing with linear probing**  
- **Collision resolution strategies**  
- Real-world hash table behavior and performance  
