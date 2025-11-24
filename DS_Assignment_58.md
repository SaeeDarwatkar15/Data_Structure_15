# 🧩 **Assignment 58: Employee Database Using Mid-Square Hashing + Linear Probing**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer and IoT Engineering  
**Subject:** Data Structures  

---

## 📝 **Problem Statement**

Write a **C++ Program** to:

- Simulate an **Employee Database** using a **Hash Table**
- Use the **Mid-Square Method** as the hash function
- Handle collisions using **Linear Probing**
- Perform:
  - Insert employee  
  - Search employee  
  - Delete employee  
  - Display entire hash table  

---

## 📘 **Theory**

### 🔹 1. **Mid-Square Hash Function**

Steps:

1. Square the key  
2. Extract the *middle digits*  
3. Reduce modulo table size  

Example:  
```
key = 101  
key² = 10201  
Middle digits = 020  
Hash index = middle % size  
```

Mid-square method is useful because:

✔ Reduces clustering  
✔ Produces uniform distribution  
✔ Works well even if keys are similar  

---

### 🔹 2. **Collision Handling — Linear Probing**

If computed index is occupied:

```
new_index = (index + i) % table_size  
where i = 1, 2, 3...
```

Simple, sequential, and efficient for moderately-filled tables.

---

### 🔹 3. **Record Deletion (Lazy Deletion)**

When deleting, you:

- Mark the slot as `<deleted>`
- Do NOT empty it completely  
- Ensures future searches still work correctly  

---

### 🔹 Time Complexity

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

struct Employee_ssd {
    int id_ssd;
    string name_ssd;
    string dept_ssd;
    bool occupied_ssd;
    bool deleted_ssd;

    Employee_ssd() {
        occupied_ssd = false;
        deleted_ssd = false;
    }
};

int midSquareHash_ssd(int key_ssd, int size_ssd) {
    long long sq_ssd = 1LL * key_ssd * key_ssd;
    int idx_ssd = (int)((sq_ssd / 10) % size_ssd); // middle digits logic
    if (idx_ssd < 0) idx_ssd = -idx_ssd;
    return idx_ssd % size_ssd;
}

int main() {
    int size_ssd;
    cout << "Enter hash table size: ";
    cin >> size_ssd;

    vector<Employee_ssd> table_ssd(size_ssd);

    while (true) {
        cout << "\n1.Insert  2.Search  3.Delete  4.Display  5.Exit\n";
        cout << "Enter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int id_ssd;
            string name_ssd, dept_ssd;
            cout << "Enter ID: ";
            cin >> id_ssd;
            cout << "Enter Name: ";
            cin >> name_ssd;
            cout << "Enter Dept: ";
            cin >> dept_ssd;

            int idx_ssd = midSquareHash_ssd(id_ssd, size_ssd);
            bool placed_ssd = false;

            for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
                int pos_ssd = (idx_ssd + i_ssd) % size_ssd;
                if (!table_ssd[pos_ssd].occupied_ssd || table_ssd[pos_ssd].deleted_ssd) {
                    table_ssd[pos_ssd].id_ssd = id_ssd;
                    table_ssd[pos_ssd].name_ssd = name_ssd;
                    table_ssd[pos_ssd].dept_ssd = dept_ssd;
                    table_ssd[pos_ssd].occupied_ssd = true;
                    table_ssd[pos_ssd].deleted_ssd = false;
                    placed_ssd = true;
                    cout << "Inserted at index " << pos_ssd << "\n";
                    break;
                }
            }
            if (!placed_ssd) cout << "Table full!\n";
        }

        else if (ch_ssd == 2) {
            int id_ssd;
            cout << "Enter ID to search: ";
            cin >> id_ssd;

            int idx_ssd = midSquareHash_ssd(id_ssd, size_ssd);
            bool found_ssd = false;

            for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
                int pos_ssd = (idx_ssd + i_ssd) % size_ssd;

                if (!table_ssd[pos_ssd].occupied_ssd && !table_ssd[pos_ssd].deleted_ssd)
                    break;

                if (table_ssd[pos_ssd].occupied_ssd && table_ssd[pos_ssd].id_ssd == id_ssd) {
                    cout << "Found at index " << pos_ssd << ": "
                         << table_ssd[pos_ssd].name_ssd << " ("
                         << table_ssd[pos_ssd].dept_ssd << ")\n";
                    found_ssd = true;
                    break;
                }
            }
            if (!found_ssd) cout << "Not found\n";
        }

        else if (ch_ssd == 3) {
            int id_ssd;
            cout << "Enter ID to delete: ";
            cin >> id_ssd;

            int idx_ssd = midSquareHash_ssd(id_ssd, size_ssd);
            bool deleted_ssd = false;

            for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
                int pos_ssd = (idx_ssd + i_ssd) % size_ssd;

                if (!table_ssd[pos_ssd].occupied_ssd && !table_ssd[pos_ssd].deleted_ssd)
                    break;

                if (table_ssd[pos_ssd].occupied_ssd && table_ssd[pos_ssd].id_ssd == id_ssd) {
                    table_ssd[pos_ssd].occupied_ssd = false;
                    table_ssd[pos_ssd].deleted_ssd = true;
                    cout << "Deleted\n";
                    deleted_ssd = true;
                    break;
                }
            }
            if (!deleted_ssd) cout << "ID not found\n";
        }

        else if (ch_ssd == 4) {
            cout << "\nIndex : ID | Name | Dept\n";
            for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
                cout << i_ssd << " : ";
                if (table_ssd[i_ssd].occupied_ssd) {
                    cout << table_ssd[i_ssd].id_ssd << " | "
                         << table_ssd[i_ssd].name_ssd << " | "
                         << table_ssd[i_ssd].dept_ssd;
                } else if (table_ssd[i_ssd].deleted_ssd) {
                    cout << "<deleted>";
                } else {
                    cout << "<empty>";
                }
                cout << "\n";
            }
        }

        else break;
    }

    return 0;
}
```

---

## 🧪 **Sample Relevant Output**

Let hash table size = **10**

Try inserting IDs:

```
Insert:
101 Rahul HR
115 Sneha IT
207 Kiran CS
Search: 115
Delete: 101
Display
```

---

### ✔ **Mid-Square Hashing Calculations**

- `101² = 10201 → middle digit 0 → index = 0`
- `115² = 13225 → middle digit 2 → index = 2`
- `207² = 42849 → middle digit 4 → index = 4`

---

### **Expected Output**

```
Inserted at index 0
Inserted at index 2
Inserted at index 4

Enter ID to search: 115
Found at index 2: Sneha (IT)

Enter ID to delete: 101
Deleted

Index : ID | Name | Dept
0 : <deleted>
1 : <empty>
2 : 115 | Sneha | IT
3 : <empty>
4 : 207 | Kiran | CS
5 : <empty>
6 : <empty>
7 : <empty>
8 : <empty>
9 : <empty>
```

---

## ✅ **Conclusion**

In this assignment you implemented:

- Mid-Square Hash Function  
- Linear Probing Collision Resolution  
- Lazy Deletion  
- Employee Database Hash Table  

It builds strong understanding of hashing, probing, and handling deletions efficiently.
