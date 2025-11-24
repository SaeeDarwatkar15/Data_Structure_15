# 🧩 **Assignment 54: Student Database Using Hash Table (Separate Chaining)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment implements a **Student Record Management System** using a **Hash Table with Separate Chaining**.  
Each student record contains **Roll Number, Name, Class, Marks**, and the hash table stores records based on the roll number.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Implement a **Hash Table** for storing student records  
- Store each student in a **linked list bucket (separate chaining)**  
- Perform operations:  
  - **Insert new student record**  
  - **Search for a student using roll number**  
  - **Delete a student record**  
  - **Display all student records bucket-wise**  

The hash function used is:

```
index = roll % table_size
```

---

## 📘 **Theory**

### 🧮 1. **Hash Tables for Student Records**

A hash table allows:

- **Fast insertion**  
- **Fast searching**  
- **Fast deletion**  
- Efficient indexing using roll numbers  

---

### 🔗 2. **Collision Handling — Separate Chaining**

If two roll numbers hash to the same bucket, they are stored in a **linked list** at that bucket:

```
Bucket 3 : [102, Saee, TE, 90] -> [109, Amit, SE, 88] -> NULL
```

### ⭐ Advantages

- Easy to implement  
- Handles large numbers of collisions  
- No need for rehashing  

### ⚠️ Disadvantages

- Slightly more memory usage  
- Linked list operations may degrade to O(n)  

---

### ⏱️ Time Complexity

| Operation | Average Case | Worst Case |
|----------|--------------|------------|
| Insert   | O(1)         | O(n)       |
| Search   | O(1)         | O(n)       |
| Delete   | O(1)         | O(n)       |

---

## 💻 **FULL C++ PROGRAM**

```cpp

#include <bits/stdc++.h>
using namespace std;

struct Student_ssd {
    int roll_ssd;
    string name_ssd;
    string class_ssd;
    int marks_ssd;

    Student_ssd(int r_ssd, string n_ssd, string c_ssd, int m_ssd) {
        roll_ssd = r_ssd;
        name_ssd = n_ssd;
        class_ssd = c_ssd;
        marks_ssd = m_ssd;
    }
};

struct StudentDB_ssd {
    int size_ssd;
    vector<list<Student_ssd>> table_ssd;

    StudentDB_ssd(int n_ssd) {
        size_ssd = n_ssd;
        table_ssd.resize(size_ssd);
    }

    int hash_ssd(int roll_ssd) {
        return roll_ssd % size_ssd;
    }

    void insert_ssd(int roll_ssd, string name_ssd, string class_ssd, int marks_ssd) {
        int idx_ssd = hash_ssd(roll_ssd);

        for (auto &s_ssd : table_ssd[idx_ssd]) {
            if (s_ssd.roll_ssd == roll_ssd) {
                s_ssd.name_ssd = name_ssd;
                s_ssd.class_ssd = class_ssd;
                s_ssd.marks_ssd = marks_ssd;
                return;
            }
        }

        table_ssd[idx_ssd].push_back(Student_ssd(roll_ssd, name_ssd, class_ssd, marks_ssd));
    }

    Student_ssd* search_ssd(int roll_ssd) {
        int idx_ssd = hash_ssd(roll_ssd);

        for (auto &s_ssd : table_ssd[idx_ssd]) {
            if (s_ssd.roll_ssd == roll_ssd)
                return &s_ssd;
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
                cout << "[" << s_ssd.roll_ssd << ", " << s_ssd.name_ssd << ", "
                     << s_ssd.class_ssd << ", " << s_ssd.marks_ssd << "] -> ";
            }
            cout << "NULL\n";
        }
    }
};

int main() {
    int n_ssd;
    cout << "Enter hash table size: ";
    cin >> n_ssd;

    StudentDB_ssd db_ssd(n_ssd);

    while (true) {
        cout << "\n1.Insert  2.Search  3.Delete  4.Display  5.Exit\n";
        cout << "Enter choice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            int roll_ssd, marks_ssd;
            string name_ssd, cls_ssd;

            cout << "Enter Roll: "; 
            cin >> roll_ssd;

            cout << "Enter Name: "; 
            cin >> name_ssd;

            cout << "Enter Class: "; 
            cin >> cls_ssd;

            cout << "Enter Marks: "; 
            cin >> marks_ssd;

            db_ssd.insert_ssd(roll_ssd, name_ssd, cls_ssd, marks_ssd);
            cout << "Record inserted.\n";
        }
        else if (ch_ssd == 2) {
            int roll_ssd;
            cout << "Enter Roll to search: ";
            cin >> roll_ssd;

            Student_ssd* s_ssd = db_ssd.search_ssd(roll_ssd);
            if (s_ssd)
                cout << "Found: " << s_ssd->roll_ssd << " " << s_ssd->name_ssd 
                     << " " << s_ssd->class_ssd << " " << s_ssd->marks_ssd << "\n";
            else
                cout << "Record not found.\n";
        }
        else if (ch_ssd == 3) {
            int roll_ssd;
            cout << "Enter Roll to delete: ";
            cin >> roll_ssd;

            cout << (db_ssd.delete_ssd(roll_ssd) ? "Record deleted\n" : "Record not found\n");
        }
        else if (ch_ssd == 4) {
            db_ssd.display_ssd();
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
Table size: 7
Insert:
101 Payal SE 85
102 Saee TE 90
103 Prasad BE 88

Search: 102
Delete: 101
Display
```

### **Output**
```
Bucket 3 : [102, Saee, TE, 90] -> NULL
Bucket 4 : [103, Prasad, BE, 88] -> NULL
...
Record 101 deleted.
```

---

## ✅ **Conclusion**

In this assignment, we implemented a **Student Database using a Hash Table (Separate Chaining)** where each bucket stores a **linked list of student records**.  
This method efficiently stores and retrieves student data using roll numbers as hash keys.  

You learned:

- How to design a **hash-based database**  
- How to perform **Insert**, **Search**, **Delete**, and **Display**  
- How **separate chaining** handles collisions  
- How hashing helps in storing structured records efficiently  

This assignment strengthens your understanding of **hashing**, **linked lists**, and **record management systems**.  
