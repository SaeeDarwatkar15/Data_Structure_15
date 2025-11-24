# 🧩 Assignment 9: Bubble Sort for Ranking Students & Roll Number Assignment

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program sorts students in **descending order of marks** using **Bubble Sort** and assigns roll numbers according to ranking (Topper = Roll No. 1). It also prints **pass-by-pass analysis** of the sorting process.

---

## 📝 Problem Statement

Write a C++ program to:

1. Input names and marks of `n` students.  
2. Sort the students in **descending order** using **Bubble Sort**.  
3. Display the array **after every sorting pass**.  
4. Assign roll numbers based on sorted marks:  
   - Highest marks → Roll No. **1**  
   - Next → Roll No. **2**, and so on  
5. Display the final sorted list with assigned roll numbers.

---

## 📘 Theory

### 🔹 Structures (`struct`)
Used to store student details:
- Name (`name_ssd`)
- Marks (`marks_ssd`)
- Roll Number (`rollNo_ssd`)

### 🔹 Dynamic Memory Allocation
Students are stored in a dynamically allocated array:
```cpp
Student_ssd* students_ssd = new Student_ssd[n_ssd];
```

### 🔹 Bubble Sort (Descending Order)
- Compares **adjacent pairs**
- Swaps them if **left < right** (to get highest first)
- Repeats until all students are sorted
- Prints **status after each pass** for analysis
- Best for conceptual understanding

### 🔹 Roll Number Assignment
After sorting:
- Index 0 → Roll No. 1  
- Index 1 → Roll No. 2  
- …and so on  

---

## 📊 Algorithm (Step-by-Step)

### Bubble Sort (Descending)

1. Start from index 0  
2. Compare `marks[j]` with `marks[j+1]`  
3. If `marks[j] < marks[j+1]`, **swap**  
4. Complete one full pass → Largest mark moves to front  
5. Repeat until sorted  
6. Display array after each pass  
7. Assign roll numbers starting from 1  

---

## 💻 Program (C++)

```cpp
#include <iostream>
using namespace std;

struct Student_ssd {
    string name_ssd;
    float marks_ssd;
    int rollNo_ssd;
};

// Bubble Sort Function
void bubbleSort_ssd(Student_ssd* students_ssd, int n_ssd) {
    for (int i_ssd = 0; i_ssd < n_ssd - 1; i_ssd++) {
        bool swapped_ssd = false;
        cout << "Pass " << i_ssd + 1 << ":\n";

        for (int j_ssd = 0; j_ssd < n_ssd - i_ssd - 1; j_ssd++) {
            if (students_ssd[j_ssd].marks_ssd < students_ssd[j_ssd + 1].marks_ssd) {
                swap(students_ssd[j_ssd], students_ssd[j_ssd + 1]);
                swapped_ssd = true;
            }
        }

        // Display after each pass
        for (int k_ssd = 0; k_ssd < n_ssd; k_ssd++) {
            cout << students_ssd[k_ssd].name_ssd
                 << " (" << students_ssd[k_ssd].marks_ssd << ")  ";
        }
        cout << "\n\n";

        if (!swapped_ssd) break; // Optimization: stop early if no swaps
    }

    // Assign Roll Numbers (Topper = Roll No. 1)
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        students_ssd[i_ssd].rollNo_ssd = i_ssd + 1;
    }
}

// Display Student List
void displayStudents_ssd(Student_ssd* students_ssd, int n_ssd) {
    cout << "Roll No.\tName\tMarks\n";
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        cout << students_ssd[i_ssd].rollNo_ssd << "\t\t"
             << students_ssd[i_ssd].name_ssd << "\t"
             << students_ssd[i_ssd].marks_ssd << endl;
    }
}

int main() {
    int n_ssd;
    cout << "Enter number of students: ";
    cin >> n_ssd;

    Student_ssd* students_ssd = new Student_ssd[n_ssd];

    // Input student details
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        cout << "Enter name and marks of student " << i_ssd + 1 << ": ";
        cin >> students_ssd[i_ssd].name_ssd >> students_ssd[i_ssd].marks_ssd;
        students_ssd[i_ssd].rollNo_ssd = 0;
    }

    cout << "\nOriginal List:\n";
    displayStudents_ssd(students_ssd, n_ssd);

    // Bubble Sort
    bubbleSort_ssd(students_ssd, n_ssd);

    cout << "\nSorted List with Roll Numbers:\n";
    displayStudents_ssd(students_ssd, n_ssd);

    delete[] students_ssd;
    return 0;
}
```

---

## 🧪 Example Execution

```
Enter number of students: 4
Enter name and marks of student 1: Saee 90
Enter name and marks of student 2: Jay 76
Enter name and marks of student 3: Ajay 99
Enter name and marks of student 4: Viay 40

Original List:
Roll No.    Name    Marks
0           Saee    90
0           Jay     76
0           Ajay    99
0           Viay    40

Pass 1:
Saee (90)  Ajay (99)  Jay (76)  Viay (40)

Pass 2:
Ajay (99)  Saee (90)  Jay (76)  Viay (40)

Pass 3:
Ajay (99)  Saee (90)  Jay (76)  Viay (40)

Sorted List with Roll Numbers:
Roll No.    Name    Marks
1           Ajay    99
2           Saee    90
3           Jay     76
4           Viay    40
```

---

## 🧠 Memory Visualization

### Dynamic Array `students_ssd` (Stored on Heap)

| Index | Name_ssd | Marks_ssd | RollNo_ssd |
|-------|----------|-----------|-------------|
| 0     | Ajay     | 99        | 1           |
| 1     | Saee     | 90        | 2           |
| 2     | Jay      | 76        | 3           |
| 3     | Viay     | 40        | 4           |

- Bubble Sort rearranges students in **descending marks**.  
- Roll numbers assigned **after sorting**: highest marks → roll no. 1.  
- Only **one dynamic array** is used.

---

## ✅ Conclusion

This assignment demonstrates:

- Use of **structures** to store student records  
- Use of **Bubble Sort** for descending ranking  
- **Pass-by-pass sorting analysis**  
- **Dynamic memory allocation** for flexible student count  
- Assignment of **roll numbers based on academic ranking**

Bubble Sort is easy to understand and ideal for showing step-by-step sorting behavior.

