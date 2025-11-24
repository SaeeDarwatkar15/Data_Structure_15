# 🧩 Assignment 6: Student Search Using Appropriate Searching Method

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
In the Computer Engg. Dept. of VIT there are **S.Y., T.Y., and B.Tech.** students.  
All students are on the ground for a function.  
We need to identify a **S.Y. Div X** student whose **name is "XYZ"** and **roll no. is 17** using an appropriate **searching method**.

This program uses **linear search** over a dynamically allocated array of students.

---

## 📝 Problem Statement

Design a **C++ program** to:

1. **Store** details of a variable number of students using dynamic memory allocation.  
2. **Search** for a specific student with:  
   - Class: `SY`  
   - Division: `X`  
   - Name: `XYZ`  
   - Roll No: `17`  
3. **Print all student records** on demand.  
4. Use an **appropriate searching method** (here: **linear search**) over the dynamic array.

---

## 📘 Theory

### 1️⃣ Dynamic Memory Allocation

- Memory for the array of students is allocated **at runtime** using:
  ```cpp
  s_ssd = new Student_ssd[n_ssd];
  ```
- Freed using:
  ```cpp
  delete[] s_ssd;
  ```

### 2️⃣ Structure (`struct`)

A `struct` is used to store student details:

- `className_ssd` → S.Y., T.Y., B.Tech  
- `division_ssd` → Division like X, B, N  
- `name_ssd` → Student name  
- `roll_ssd` → Roll number  

### 3️⃣ Searching Method: Linear Search

- We use **linear search** because:
  - Data is small and unsorted.
  - We check each record one by one.
- For each student, we compare:
  - Class  
  - Division  
  - Name  
  - Roll number  

If all match, the student is found.

### 4️⃣ Menu Driven Program

- Allows user to pick operations:
  - Search for specific student (`SY X XYZ 17`)  
  - Print all records  
  - Exit  

---

## 📊 Algorithm (Step-by-Step)

1. **Input** number of students `n_ssd`.  
2. Dynamically allocate an array of `Student_ssd` of size `n_ssd`.  
3. For each student:
   - Input `className_ssd` (SY/TY/BTech).  
   - Input `division_ssd`.  
   - Input `name_ssd`.  
   - Input `roll_ssd`.  
4. Enter a **menu loop**:
   - **Option 1:** Search for student with (`SY`, `X`, `"XYZ"`, `17`).  
     - Set `found_ssd = false`.  
     - For each student in array:
       - If all four fields match, print student details and set `found_ssd = true`.  
       - Break loop.  
     - If not found, print "Student Not Found."  
   - **Option 2:** Print all student records.  
   - **Option 0:** Exit.  
   - For any other choice, print "Invalid choice!".  
5. Repeat menu until user chooses `0`.  
6. Free dynamically allocated memory using `delete[] s_ssd`.  
7. End program.

---

## 💻 CODE (C++)

```cpp
#include <iostream>
#include <string>
using namespace std;

// Structure to store student information
struct Student_ssd {
    string className_ssd;
    string division_ssd;
    string name_ssd;
    int roll_ssd;
};

int main() {
    int n_ssd, i_ssd, choice_ssd;
    Student_ssd* s_ssd = nullptr;

    // Input number of students
    cout << "Enter number of students: ";
    cin >> n_ssd;

    // Dynamic allocation of students
    s_ssd = new Student_ssd[n_ssd];

    // Input student details
    for (i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        cout << "\nEnter details of student " << i_ssd + 1 << "\n";
        cout << "Class (SY/TY/BTech): ";
        cin >> s_ssd[i_ssd].className_ssd;
        cout << "Division: ";
        cin >> s_ssd[i_ssd].division_ssd;
        cout << "Name: ";
        cin >> s_ssd[i_ssd].name_ssd;
        cout << "Roll No: ";
        cin >> s_ssd[i_ssd].roll_ssd;
    }

    // Menu loop
    do {
        cout << "\n--- MENU ---\n";
        cout << "1) Search for SY Div X Name XYZ Roll 17\n";
        cout << "2) Print All Records\n";
        cout << "0) Exit\n";
        cout << "Enter choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1: {
                bool found_ssd = false;
                for (i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
                    if (s_ssd[i_ssd].className_ssd == "SY" &&
                        s_ssd[i_ssd].division_ssd == "X" &&
                        s_ssd[i_ssd].name_ssd == "XYZ" &&
                        s_ssd[i_ssd].roll_ssd == 17) {
                        cout << "\nStudent Found:\n";
                        cout << "Class: " << s_ssd[i_ssd].className_ssd
                             << " | Division: " << s_ssd[i_ssd].division_ssd
                             << " | Name: " << s_ssd[i_ssd].name_ssd
                             << " | Roll: " << s_ssd[i_ssd].roll_ssd << endl;
                        found_ssd = true;
                        break;
                    }
                }
                if (!found_ssd) {
                    cout << "\nStudent Not Found.\n";
                }
                break;
            }

            case 2: {
                cout << "\nAll Student Records:\n";
                for (i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
                    cout << "Class: " << s_ssd[i_ssd].className_ssd
                         << " | Division: " << s_ssd[i_ssd].division_ssd
                         << " | Name: " << s_ssd[i_ssd].name_ssd
                         << " | Roll: " << s_ssd[i_ssd].roll_ssd << endl;
                }
                break;
            }

            case 0:
                cout << "Exiting...\n";
                break;

            default:
                cout << "Invalid choice!\n";
        }
    } while (choice_ssd != 0);

    // Free memory
    delete[] s_ssd;
    return 0;
}
```

---

## 🧪 Example Execution

**Input / Output:**

```
Enter number of students: 3

Enter details of student 1
Class (SY/TY/BTech): SY
Division: X
Name: XYZ
Roll No: 17

Enter details of student 2
Class (SY/TY/BTech): TY
Division: B
Name: ABC
Roll No: 29

Enter details of student 3
Class (SY/TY/BTech): BTech
Division: N
Name: MNO
Roll No: 20

--- MENU ---
1) Search for SY Div X Name XYZ Roll 17
2) Print All Records
0) Exit
Enter choice: 1

Student Found:
Class: SY | Division: X | Name: XYZ | Roll: 17

--- MENU ---
1) Search for SY Div X Name XYZ Roll 17
2) Print All Records
0) Exit
Enter choice: 2

All Student Records:
Class: SY | Division: X | Name: XYZ | Roll: 17
Class: TY | Division: B | Name: ABC | Roll: 29
Class: BTech | Division: N | Name: MNO | Roll: 20

--- MENU ---
1) Search for SY Div X Name XYZ Roll 17
2) Print All Records
0) Exit
Enter choice: 0
Exiting...
```

---

## 🧠 Memory Visualization

### Student Array (`s_ssd`) Layout in Heap

| Index | className_ssd | division_ssd | name_ssd | roll_ssd |
|-------|---------------|--------------|----------|----------|
| 0     | SY            | X            | XYZ      | 17       |
| 1     | TY            | B            | ABC      | 29       |
| 2     | BTech         | N            | MNO      | 20       |

- `s_ssd` is a **dynamically allocated array** of `Student_ssd`.  
- Each element of `s_ssd` holds one complete student record.  
- The **menu** uses this array for:
  - **Searching** (Option 1): Linear scan with full field comparison.  
  - **Displaying all students** (Option 2).

---

## ✅ Conclusion

This assignment demonstrates:

- How to use a **structure** to store multiple fields of student information.  
- How to use **dynamic memory allocation** (`new` and `delete[]`) to handle a variable number of records.  
- How to apply a simple and effective **linear search** to find a specific student:
  - Class = `SY`  
  - Division = `X`  
  - Name = `XYZ`  
  - Roll No = `17`  
- How to design a **menu-driven C++ program** for basic record management.

This example connects **data structures**, **searching techniques**, and **dynamic memory** in a practical student record use-case.

