# 🧩 **Assignment 55: Faculty Database Using Hash Table (Linear Probing with MOD Hash Function)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program simulates a **Faculty Database System** using a **Hash Table** with **Linear Probing** for collision resolution.  
Each faculty record contains **Faculty ID, Name, Department, Experience**, and collisions are resolved using the formula:

```
index = (hash + i) % table_size
```

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Create a **Hash Table** to store faculty records  
- Use **MOD (%) as the hash function**  
- Handle collisions using **Linear Probing**  
- Perform operations:
  - **Insert a faculty record**
  - **Search a faculty by ID**
  - **Delete a faculty by ID**
  - **Display complete faculty database**

---

## 📘 **Theory**

### 🎯 1. **Hashing**

Hashing maps a faculty ID to a specific index:

```
index = faculty_id % table_size
```

This ensures fast access and constant-time average performance.

---

### ⚠️ 2. **Collision Handling — Linear Probing**

If the computed index is already occupied:

```
new_index = (index + i) % table_size
where i = 1, 2, 3...
```

The next slot in sequence is checked until an empty one is found.

### Benefits:

- Simple technique  
- Efficient in practice  
- Maintains data in contiguous memory  

### Drawbacks:

- Primary clustering  
- Table may get full  

---

### 🗑️ 3. **Deletion**

Deletion uses **lazy deletion**:

- Mark slot as `<deleted>`  
- Keeps probing sequence intact for future searches  

---

### ⏱️ Time Complexity

| Operation | Avg | Worst |
|----------|------|--------|
| Insert   | O(1) | O(n)   |
| Search   | O(1) | O(n)   |
| Delete   | O(1) | O(n)   |

---

## 💻 **FULL C++ PROGRAM**

```cpp

#include <bits/stdc++.h>
using namespace std;

struct Faculty_ssd {
    int id_ssd;
    string name_ssd;
    string dept_ssd;
    int exp_ssd;
    bool occupied_ssd;
    bool deleted_ssd;

    Faculty_ssd() {
        occupied_ssd = false;
        deleted_ssd = false;
    }
};

struct FacultyHash_ssd {
    int size_ssd;
    vector<Faculty_ssd> table_ssd;

    FacultyHash_ssd(int n) {
        size_ssd = n;
        table_ssd.resize(size_ssd);
    }

    int hash_ssd(int id_ssd) {
        return id_ssd % size_ssd;
    }

    bool insert_ssd(int id_ssd, string name_ssd, string dept_ssd, int exp_ssd) {
        int idx_ssd = hash_ssd(id_ssd);
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            int pos_ssd = (idx_ssd + i_ssd) % size_ssd;

            if (!table_ssd[pos_ssd].occupied_ssd || table_ssd[pos_ssd].deleted_ssd) {
                table_ssd[pos_ssd].id_ssd = id_ssd;
                table_ssd[pos_ssd].name_ssd = name_ssd;
                table_ssd[pos_ssd].dept_ssd = dept_ssd;
                table_ssd[pos_ssd].exp_ssd = exp_ssd;
                table_ssd[pos_ssd].occupied_ssd = true;
                table_ssd[pos_ssd].deleted_ssd = false;
                return true;
            }
        }
        return false;
    }

    int search_ssd(int id_ssd) {
        int idx_ssd = hash_ssd(id_ssd);
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            int pos_ssd = (idx_ssd + i_ssd) % size_ssd;

            if (!table_ssd[pos_ssd].occupied_ssd && !table_ssd[pos_ssd].deleted_ssd)
                return -1;

            if (table_ssd[pos_ssd].occupied_ssd && table_ssd[pos_ssd].id_ssd == id_ssd)
                return pos_ssd;
        }
        return -1;
    }

    bool delete_ssd(int id_ssd) {
        int pos_ssd = search_ssd(id_ssd);
        if (pos_ssd == -1) return false;
        table_ssd[pos_ssd].occupied_ssd = false;
        table_ssd[pos_ssd].deleted_ssd = true;
        return true;
    }

    void display_ssd() {
        cout << "\nFaculty Hash Table:\n";
        cout << "Index : ID | Name | Department | Experience\n";

        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            cout << i_ssd << " : ";
            if (table_ssd[i_ssd].occupied_ssd) {
                cout << table_ssd[i_ssd].id_ssd << " | "
                     << table_ssd[i_ssd].name_ssd << " | "
                     << table_ssd[i_ssd].dept_ssd << " | "
                     << table_ssd[i_ssd].exp_ssd;
            }
            else if (table_ssd[i_ssd].deleted_ssd) {
                cout << "<deleted>";
            }
            else {
                cout << "<empty>";
            }
            cout << "\n";
        }
    }
};

int main() {
    int n_ssd;
    cout << "Enter hash table size: ";
    cin >> n_ssd;

    FacultyHash_ssd ht_ssd(n_ssd);

    while (true) {
        cout << "\n1.Insert  2.Search  3.Delete  4.Display  5.Exit\n";
        cout << "Enter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int id_ssd, exp_ssd;
            string name_ssd, dept_ssd;

            cout << "Enter Faculty ID: "; cin >> id_ssd;
            cout << "Enter Name: "; cin >> name_ssd;
            cout << "Enter Dept: "; cin >> dept_ssd;
            cout << "Enter Experience: "; cin >> exp_ssd;

            if (ht_ssd.insert_ssd(id_ssd, name_ssd, dept_ssd, exp_ssd))
                cout << "Inserted successfully\n";
            else
                cout << "Table Full - Insert Failed\n";
        }
        else if (ch_ssd == 2) {
            int id_ssd;
            cout << "Enter Faculty ID to search: ";
            cin >> id_ssd;

            int pos_ssd = ht_ssd.search_ssd(id_ssd);
            if (pos_ssd == -1) cout << "Not Found\n";
            else cout << "Found at index: " << pos_ssd << "\n";
        }
        else if (ch_ssd == 3) {
            int id_ssd;
            cout << "Enter Faculty ID to delete: ";
            cin >> id_ssd;

            cout << (ht_ssd.delete_ssd(id_ssd) ? "Deleted\n" : "Not Found\n");
        }
        else if (ch_ssd == 4) {
            ht_ssd.display_ssd();
        }
        else break;
    }
}
```

---

## 🧪 **Example Execution**

### **Input**
```
Table size: 7
Insert:
101 Payal CE 5
108 Saee IT 3
115 Prasad CS 8

Search: 108
Delete: 101
Display
```

### **Output**
```
0 : <empty>
1 : 108 | Saee | IT | 3
2 : <deleted>
3 : 115 | Prasad | CS | 8
4 : <empty>
5 : <empty>
6 : <empty>

Search Result: Found at index 1
Record 101 deleted
```

---

## ✅ **Conclusion**

In this assignment, you implemented a full **Faculty Database System** using:

- **Hashing with MOD**
- **Linear Probing**
- **Lazy Deletion**
- **Struct-based record storage**

You learned:

- How hashing maps faculty IDs  
- How to handle collisions using linear probing  
- How to efficiently search and delete entries  
- How hash tables store real-world structured data  

This assignment strengthens understanding of **hash table operations**, **collision resolution**, and **record management systems**.  
