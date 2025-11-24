# 🧩 **Assignment 59: Student Database Using Hash Table with Separate Chaining**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

---

## 📝 Problem Statement

Write a **C++ program** to simulate a **Student Database Management System** using **Hashing** that allows:

- Efficient **Insertion**
- Fast **Search**
- Quick **Deletion**
- Display of full database

Key Features:
- Hash Table implemented using **Separate Chaining**
- Avoids collision by storing multiple records in the same bucket using linked lists
- Each record stores:
  - **Roll number**
  - **Name**
  - **Branch**
  - **CGPA**

---

## 📘 Theory

### ✔ Hash Function (Modulo Method)
```
index = roll_no % table_size
```
Advantages:
- Simple and fast
- Distributes values fairly well

---

### ✔ Collision Handling using Separate Chaining

If two keys hash to the same index:
- Store them in the **same bucket**
- Using a **linked list** (here implemented via STL `list`)

Benefits:
- No clustering issues
- Table can grow logically without extra probing

---

### ⏱️ Time Complexity

| Operation | Average | Worst Case |
|----------|---------|------------|
| Insert   | O(1)    | O(n)       |
| Search   | O(1)    | O(n)       |
| Delete   | O(1)    | O(n)       |

---

## 💻 FULL C++ PROGRAM

```cpp

#include <bits/stdc++.h>
using namespace std;

struct Student_ssd {
    int roll_ssd;
    string name_ssd;
    string branch_ssd;
    float cgpa_ssd;

    Student_ssd(int r_ssd, string n_ssd, string b_ssd, float c_ssd) {
        roll_ssd = r_ssd;
        name_ssd = n_ssd;
        branch_ssd = b_ssd;
        cgpa_ssd = c_ssd;
    }
};

struct StudentHash_ssd {
    int size_ssd;
    vector<list<Student_ssd>> table_ssd;

    StudentHash_ssd(int n_ssd) {
        size_ssd = n_ssd;
        table_ssd.resize(size_ssd);
    }

    int hash_ssd(int roll_ssd) {
        return roll_ssd % size_ssd;
    }

    void insert_ssd(int roll_ssd, string name_ssd, string branch_ssd, float cgpa_ssd) {
        int idx_ssd = hash_ssd(roll_ssd);
        for (auto &s_ssd : table_ssd[idx_ssd]) {
            if (s_ssd.roll_ssd == roll_ssd) {
                s_ssd.name_ssd = name_ssd;
                s_ssd.branch_ssd = branch_ssd;
                s_ssd.cgpa_ssd = cgpa_ssd;
                return;
            }
        }
        table_ssd[idx_ssd].push_back(Student_ssd(roll_ssd, name_ssd, branch_ssd, cgpa_ssd));
    }

    Student_ssd* search_ssd(int roll_ssd) {
        int idx_ssd = hash_ssd(roll_ssd);
        for (auto &s_ssd : table_ssd[idx_ssd]) {
            if (s_ssd.roll_ssd == roll_ssd) return &s_ssd;
        }
        return nullptr;
    }

    bool delete_ssd(int roll_ssd) {
        int idx_ssd = hash_ssd(roll_ssd);
        auto &lst_ssd = table_ssd[idx_ssd];
        for (auto it_ssd = lst_ssd.begin(); it_ssd != lst_ssd.end(); ++it_ssd) {
            if (it_ssd->roll_ssd == roll_ssd) {
                lst_ssd.erase(it_ssd);
                return true;
            }
        }
        return false;
    }

    void display_ssd() {
        cout << "\nStudent Hash Table:\n";
        for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
            cout << "Bucket " << i_ssd << " : ";
            for (auto &s_ssd : table_ssd[i_ssd]) {
                cout << "[" << s_ssd.roll_ssd << "," << s_ssd.name_ssd
                     << "," << s_ssd.branch_ssd << "," << s_ssd.cgpa_ssd << "] -> ";
            }
            cout << "NULL\n";
        }
    }
};

int main() {
    int size_ssd;
    cout << "Enter hash table size: ";
    cin >> size_ssd;

    StudentHash_ssd db_ssd(size_ssd);

    while (true) {
        cout << "\n1.Insert  2.Search  3.Delete  4.Display  5.Exit\n";
        cout << "Enter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int roll_ssd;
            string name_ssd, branch_ssd;
            float cgpa_ssd;
            cout << "Roll: "; cin >> roll_ssd;
            cout << "Name: "; cin >> name_ssd;
            cout << "Branch: "; cin >> branch_ssd;
            cout << "CGPA: "; cin >> cgpa_ssd;
            db_ssd.insert_ssd(roll_ssd, name_ssd, branch_ssd, cgpa_ssd);
            cout << "Inserted.\n";
        }
        else if (ch_ssd == 2) {
            int roll_ssd;
            cout << "Enter roll to search: ";
            cin >> roll_ssd;
            Student_ssd* s_ssd = db_ssd.search_ssd(roll_ssd);
            if (s_ssd)
                cout << "Found: " << s_ssd->roll_ssd << " " << s_ssd->name_ssd << " "
                     << s_ssd->branch_ssd << " " << s_ssd->cgpa_ssd << "\n";
            else
                cout << "Not found\n";
        }
        else if (ch_ssd == 3) {
            int roll_ssd;
            cout << "Enter roll to delete: ";
            cin >> roll_ssd;
            cout << (db_ssd.delete_ssd(roll_ssd) ? "Deleted\n" : "Not found\n");
        }
        else if (ch_ssd == 4) {
            db_ssd.display_ssd();
        }
        else break;
    }
    return 0;
}
```

---

## 🧪 Example Execution Output

**Input**
```
Enter hash table size: 5

Insert:
Roll: 101 Name: Payal Branch: CSE CGPA: 9.1
Roll: 102 Name: Saee Branch: IT  CGPA: 8.7
Search: 101
Delete: 102
Display
```

---

**Output**
```
Inserted.
Inserted.
Found: 101 Payal CSE 9.1
Deleted

Student Hash Table:
Bucket 0 : NULL
Bucket 1 : [101,Payal,CSE,9.1] -> NULL
Bucket 2 : NULL
Bucket 3 : NULL
Bucket 4 : NULL
```

---

## ✅ Conclusion

✔ Implemented **Student Database** using **Hash Table**  
✔ Used **Separate Chaining** for collision resolution  
✔ Performed **Insert, Search, Delete, Display** operations successfully  
✔ Demonstrated practical application of hashing in record management  

---
