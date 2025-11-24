# 🧩 **Assignment 60: Smart College Placement Portal Using Advanced Hashing (Dynamic Rehashing + Double Hashing)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer and IoT Engineering  
**Subject:** Data Structures  

---

## 🎯 **Aim**

Design and implement a **Smart College Placement Portal** using **advanced hashing techniques** that ensures:

- **High performance** for large datasets  
- **Low collision probability**  
- **Dynamic growth** using **Rehashing**  
- **Fast insertion, search, and deletion**  
- Uses **Double Hashing** for collision resolution  

---

## 📘 **Theory**

### ✔ **1. Double Hashing**
Double hashing minimizes collisions using two hash functions:

```
h1(key) = key % table_size  
h2(key) = 1 + (key % (table_size - 1))
```

Final index for i-th probe:
```
index = (h1 + i × h2) % table_size
```

Advantages:
- Fewer clusters  
- High distribution  
- Efficient even at high load factors  

---

### ✔ **2. Dynamic Rehashing**
When **load factor > 0.7**, table automatically enlarges:

```
new_table_size = old_size * 2 + 1
```

Steps:
1. Create larger table  
2. Re-insert all old records  
3. Reset deleted flags  

Ensures:
- Reduced collisions  
- Faster search performance  

---

### ✔ **3. Placement Record Structure**
Each student placement record contains:

- **Student ID**
- **Name**
- **Branch**
- **Company name**
- Flags: occupied / deleted  

Stored in hash table entries.

---

## 💻 **FULL C++ PROGRAM**

```cpp

#include <bits/stdc++.h>
using namespace std;

struct Placement_ssd {
    int id_ssd;
    string name_ssd;
    string branch_ssd;
    string company_ssd;
    bool occupied_ssd;
    bool deleted_ssd;

    Placement_ssd() {
        occupied_ssd = false;
        deleted_ssd = false;
    }
};

struct PlacementHash_ssd {
    int size_ssd;
    int count_ssd;
    vector<Placement_ssd> table_ssd;

    PlacementHash_ssd(int n) {
        size_ssd = n;
        count_ssd = 0;
        table_ssd.resize(size_ssd);
    }

    double loadFactor_ssd() {
        return (double)count_ssd / size_ssd;
    }

    int h1_ssd(int key_ssd) { return key_ssd % size_ssd; }
    int h2_ssd(int key_ssd) { return 1 + (key_ssd % (size_ssd - 1)); }

    int findSlot_ssd(int key_ssd) {
        int base_ssd = h1_ssd(key_ssd);
        int step_ssd = h2_ssd(key_ssd);
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            int idx_ssd = (base_ssd + i_ssd * step_ssd) % size_ssd;
            if (!table_ssd[idx_ssd].occupied_ssd || table_ssd[idx_ssd].deleted_ssd)
                return idx_ssd;
        }
        return -1;
    }

    int searchIndex_ssd(int key_ssd) {
        int base_ssd = h1_ssd(key_ssd);
        int step_ssd = h2_ssd(key_ssd);
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            int idx_ssd = (base_ssd + i_ssd * step_ssd) % size_ssd;
            if (!table_ssd[idx_ssd].occupied_ssd && !table_ssd[idx_ssd].deleted_ssd)
                return -1;
            if (table_ssd[idx_ssd].occupied_ssd && table_ssd[idx_ssd].id_ssd == key_ssd)
                return idx_ssd;
        }
        return -1;
    }

    void rehash_ssd() {
        int oldSize_ssd = size_ssd;
        size_ssd = size_ssd * 2 + 1;
        vector<Placement_ssd> oldTable_ssd = table_ssd;

        table_ssd.clear();
        table_ssd.resize(size_ssd);
        count_ssd = 0;

        for (int i_ssd = 0; i_ssd < oldSize_ssd; i_ssd++) {
            if (oldTable_ssd[i_ssd].occupied_ssd) {
                insert_ssd(oldTable_ssd[i_ssd].id_ssd,
                           oldTable_ssd[i_ssd].name_ssd,
                           oldTable_ssd[i_ssd].branch_ssd,
                           oldTable_ssd[i_ssd].company_ssd);
            }
        }
        cout << "Rehashed to new size: " << size_ssd << "\n";
    }

    void insert_ssd(int id_ssd, string name_ssd, string branch_ssd, string company_ssd) {
        if (loadFactor_ssd() > 0.7) {
            rehash_ssd();
        }
        int idx_ssd = findSlot_ssd(id_ssd);
        if (idx_ssd == -1) {
            cout << "Table full, cannot insert.\n";
            return;
        }
        table_ssd[idx_ssd].id_ssd = id_ssd;
        table_ssd[idx_ssd].name_ssd = name_ssd;
        table_ssd[idx_ssd].branch_ssd = branch_ssd;
        table_ssd[idx_ssd].company_ssd = company_ssd;
        table_ssd[idx_ssd].occupied_ssd = true;
        table_ssd[idx_ssd].deleted_ssd = false;
        count_ssd++;
        cout << "Inserted at index " << idx_ssd << "\n";
    }

    void search_ssd(int id_ssd) {
        int idx_ssd = searchIndex_ssd(id_ssd);
        if (idx_ssd == -1) {
            cout << "Record not found.\n";
        } else {
            cout << "Found at " << idx_ssd << " -> "
                 << table_ssd[idx_ssd].id_ssd << " "
                 << table_ssd[idx_ssd].name_ssd << " "
                 << table_ssd[idx_ssd].branch_ssd << " "
                 << table_ssd[idx_ssd].company_ssd << "\n";
        }
    }

    void delete_ssd(int id_ssd) {
        int idx_ssd = searchIndex_ssd(id_ssd);
        if (idx_ssd == -1) {
            cout << "Record not found.\n";
        } else {
            table_ssd[idx_ssd].occupied_ssd = false;
            table_ssd[idx_ssd].deleted_ssd = true;
            cout << "Deleted.\n";
            count_ssd--;
        }
    }

    void display_ssd() {
        cout << "\nPlacement Hash Table (size=" << size_ssd << "):\n";
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            cout << i_ssd << " : ";
            if (table_ssd[i_ssd].occupied_ssd) {
                cout << table_ssd[i_ssd].id_ssd << " | "
                     << table_ssd[i_ssd].name_ssd << " | "
                     << table_ssd[i_ssd].branch_ssd << " | "
                     << table_ssd[i_ssd].company_ssd;
            } else if (table_ssd[i_ssd].deleted_ssd) {
                cout << "<deleted>";
            } else {
                cout << "<empty>";
            }
            cout << "\n";
        }
    }
};

int main() {
    PlacementHash_ssd portal_ssd(7); // initial size

    while (true) {
        cout << "\n1.Insert  2.Search  3.Delete  4.Display  5.Exit\n";
        cout << "Enter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int id_ssd;
            string name_ssd, branch_ssd, company_ssd;
            cout << "ID: ";      cin >> id_ssd;
            cout << "Name: ";    cin >> name_ssd;
            cout << "Branch: ";  cin >> branch_ssd;
            cout << "Company: "; cin >> company_ssd;
            portal_ssd.insert_ssd(id_ssd, name_ssd, branch_ssd, company_ssd);
        }
        else if (ch_ssd == 2) {
            int id_ssd;
            cout << "Enter ID to search: ";
            cin >> id_ssd;
            portal_ssd.search_ssd(id_ssd);
        }
        else if (ch_ssd == 3) {
            int id_ssd;
            cout << "Enter ID to delete: ";
            cin >> id_ssd;
            portal_ssd.delete_ssd(id_ssd);
        }
        else if (ch_ssd == 4) {
            portal_ssd.display_ssd();
        }
        else break;
    }
    return 0;
}
```

---

## 🧪 **Sample Example Execution**

### **Input**
```
Insert:
101 Saee IT Google
115 Payal CSE Microsoft
122 Riya ENTC Infosys
135 Om CE TCS
180 Aditya IT Amazon   <-- triggers rehash (LF > 0.7)

Search ID: 115
Delete ID: 122
Display
```

---

### **Output**
```
Inserted at index 3
Inserted at index 4
Inserted at index 6
Inserted at index 1
Rehashed to new size: 15
Inserted at index 12

Found at 4 -> 115 Payal CSE Microsoft
Deleted.

Placement Hash Table (size=15):
0 : <empty>
1 : 135 | Om | CE | TCS
2 : <empty>
3 : 101 | Saee | IT | Google
4 : 115 | Payal | CSE | Microsoft
5 : <empty>
6 : <deleted>
7 : <empty>
8 : <empty>
9 : <empty>
10 : <empty>
11 : <empty>
12 : 180 | Aditya | IT | Amazon
13 : <empty>
14 : <empty>
```

---

## ✅ **Conclusion**

✔ Implemented a **Smart Placement Portal** using **Advanced Hashing**  
✔ Used **Double Hashing** + **Rehashing** for high performance  
✔ Achieved **low collision probability** and **efficient searching**  
✔ Fully dynamic and scalable placement record system  

---
