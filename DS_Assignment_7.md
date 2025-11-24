# 🧩 Assignment 7: Bubble Sort and Quick Sort on 1D Array of Student Structure

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Bubble Sort** and **Quick Sort** on a **1D array of `Student` structure** with sorting **key as `student_roll_no`**, and counts the **number of swaps** performed by each method.

---

## 📝 Problem Statement

Write a C++ program to:

1. Generate **random student records** (name, roll number, total marks).  
2. Sort the students by **roll number** using **Bubble Sort**, showing **pass-wise swap details**.  
3. Sort the students by **roll number** using **Quick Sort**.  
4. Display the **number of swaps** performed by each sorting algorithm for comparison.

Each student record contains:
- `student_name`  
- `student_roll_no`  
- `total_marks`  

---

## 📘 Theory

### 1️⃣ Dynamic Memory Allocation

- Arrays of students are allocated at **runtime** using:
  ```cpp
  Student_ssd* arr1_ssd = new Student_ssd[n_ssd];
  Student_ssd* arr2_ssd = new Student_ssd[n_ssd];
  ```
- Freed using:
  ```cpp
  delete[] arr1_ssd;
  delete[] arr2_ssd;
  ```

### 2️⃣ Structure (`struct`)

The `Student_ssd` structure stores:
- `name_ssd` → Student name  
- `roll_no_ssd` → Roll number (sorting key)  
- `total_marks_ssd` → Total marks  

### 3️⃣ Sorting Algorithms

#### 🔹 Bubble Sort

- Simple comparison-based sorting.
- Repeatedly compares **adjacent elements**.
- Swaps if they are in the wrong order.
- Good for **pass-wise tracing** and understanding basic sorting.
- Time complexity approx: **O(n²)**.

#### 🔹 Quick Sort

- **Divide-and-conquer** sorting algorithm.  
- Uses a **pivot** to partition array into two subarrays:
  - Elements **less than pivot** on left  
  - Elements **greater than pivot** on right  
- Recursively sorts the subarrays.  
- Average time complexity: **O(n log n)**.

### 4️⃣ Swap Counting

- Every time two student records are swapped, a **swap counter** is incremented.
- This helps to **compare efficiency** (in terms of swaps) between Bubble Sort and Quick Sort.

---

## 📊 Algorithm (Step-by-Step)

### A) Bubble Sort (Key: `roll_no_ssd`)

1. Set `swapCount_ssd = 0`.  
2. For each **pass** from `0` to `size_ssd - 2`:
   - Set `swapped_ssd = false`.  
   - For each index `j` from `0` to `size_ssd - pass - 2`:
     - If `arr_ssd[j].roll_no_ssd > arr_ssd[j + 1].roll_no_ssd`:
       - Swap the two elements using `swap_ssd()`.  
       - Increment `swapCount_ssd`.  
       - Set `swapped_ssd = true`.  
   - Print array after each pass.  
   - If `swapped_ssd == false` → array already sorted → break early.

---

### B) Quick Sort (Key: `roll_no_ssd`)

1. Choose **last element** as pivot (`pivot_ssd = arr_ssd[last_ssd]`).  
2. Initialize `i_ssd = first_ssd - 1`.  
3. For each element `j_ssd` from `first_ssd` to `last_ssd - 1`:
   - If `arr_ssd[j_ssd].roll_no_ssd < pivot_ssd.roll_no_ssd`:
     - Increment `i_ssd`.  
     - Swap `arr_ssd[i_ssd]` and `arr_ssd[j_ssd]`, increment swap counter.  
4. Finally, place pivot in correct position:
   - Swap `arr_ssd[i_ssd + 1]` and `arr_ssd[last_ssd]`, increment swap counter.  
5. Return pivot index `pi_ssd = i_ssd + 1`.  
6. Recursively apply Quick Sort to:
   - Left part: `first_ssd` to `pi_ssd - 1`  
   - Right part: `pi_ssd + 1` to `last_ssd`  

---

## 💻 CODE (C++)

```cpp
#include <iostream>
#include <string>
#include <cstdlib>
using namespace std;

#define NAME_LEN_ssd 50

struct Student_ssd {
    string name_ssd;
    int roll_no_ssd;
    int total_marks_ssd;
};

// Function prototypes
void generateRandomStudents_ssd(Student_ssd* arr_ssd, int size_ssd);
void printStudents_ssd(Student_ssd* arr_ssd, int size_ssd);
void bubbleSort_ssd(Student_ssd* arr_ssd, int size_ssd, int* swapCount_ssd);
void quickSort_ssd(Student_ssd* arr_ssd, int first_ssd, int last_ssd, int* swapCount_ssd);
int partition_ssd(Student_ssd* arr_ssd, int first_ssd, int last_ssd, int* swapCount_ssd);
void swap_ssd(Student_ssd* a_ssd, Student_ssd* b_ssd, int* swapCount_ssd);

int main() {
    int n_ssd;
    cout << "Enter number of students: ";
    cin >> n_ssd;

    Student_ssd* arr1_ssd = new Student_ssd[n_ssd];
    Student_ssd* arr2_ssd = new Student_ssd[n_ssd];

    generateRandomStudents_ssd(arr1_ssd, n_ssd);
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++)
        arr2_ssd[i_ssd] = arr1_ssd[i_ssd];

    int bubbleSwaps_ssd = 0;
    int quickSwaps_ssd = 0;

    cout << "\nOriginal Student Array:\n";
    printStudents_ssd(arr1_ssd, n_ssd);

    // Bubble Sort
    bubbleSort_ssd(arr1_ssd, n_ssd, &bubbleSwaps_ssd);
    cout << "\nAfter Bubble Sort (by Roll No):\n";
    printStudents_ssd(arr1_ssd, n_ssd);
    cout << "Number of swaps in Bubble Sort: " << bubbleSwaps_ssd << endl;

    // Quick Sort
    quickSort_ssd(arr2_ssd, 0, n_ssd - 1, &quickSwaps_ssd);
    cout << "\nAfter Quick Sort (by Roll No):\n";
    printStudents_ssd(arr2_ssd, n_ssd);
    cout << "Number of swaps in Quick Sort: " << quickSwaps_ssd << endl;

    delete[] arr1_ssd;
    delete[] arr2_ssd;
    return 0;
}

// Generate random students
void generateRandomStudents_ssd(Student_ssd* arr_ssd, int size_ssd) {
    const string names_ssd[] = {"Abir","Aarav","Isha","Rohan","Priya","Vikas","Neha","Sahil","Anaya","Dev"};
    int N_NAMES_ssd = sizeof(names_ssd) / sizeof(names_ssd[0]);
    for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
        arr_ssd[i_ssd].name_ssd = names_ssd[rand() % N_NAMES_ssd];
        arr_ssd[i_ssd].roll_no_ssd = rand() % 100 + 1;
        arr_ssd[i_ssd].total_marks_ssd = rand() % 101;
    }
}

// Print student array
void printStudents_ssd(Student_ssd* arr_ssd, int size_ssd) {
    cout << "Roll No\tName\tTotal Marks\n";
    for (int i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
        cout << arr_ssd[i_ssd].roll_no_ssd << "\t"
             << arr_ssd[i_ssd].name_ssd << "\t"
             << arr_ssd[i_ssd].total_marks_ssd << "\n";
    }
}

// Swap two students and count swaps
void swap_ssd(Student_ssd* a_ssd, Student_ssd* b_ssd, int* swapCount_ssd) {
    Student_ssd temp_ssd = *a_ssd;
    *a_ssd = *b_ssd;
    *b_ssd = temp_ssd;
    (*swapCount_ssd)++;
}

// Bubble Sort with pass-wise details
void bubbleSort_ssd(Student_ssd* arr_ssd, int size_ssd, int* swapCount_ssd) {
    *swapCount_ssd = 0;
    bool swapped_ssd;
    for (int i_ssd = 0; i_ssd < size_ssd - 1; i_ssd++) {
        swapped_ssd = false;
        cout << "\n--- Pass " << i_ssd + 1 << " ---\n";
        for (int j_ssd = 0; j_ssd < size_ssd - i_ssd - 1; j_ssd++) {
            if (arr_ssd[j_ssd].roll_no_ssd > arr_ssd[j_ssd + 1].roll_no_ssd) {
                cout << "Swapping Roll " << arr_ssd[j_ssd].roll_no_ssd << " ("
                     << arr_ssd[j_ssd].name_ssd << ") and Roll "
                     << arr_ssd[j_ssd + 1].roll_no_ssd << " ("
                     << arr_ssd[j_ssd + 1].name_ssd << ")\n";
                swap_ssd(&arr_ssd[j_ssd], &arr_ssd[j_ssd + 1], swapCount_ssd);
                swapped_ssd = true;
            }
        }
        cout << "Array after Pass " << i_ssd + 1 << ":\n";
        printStudents_ssd(arr_ssd, size_ssd);
        if (!swapped_ssd) {
            cout << "No swaps in this pass, array is already sorted.\n";
            break;
        }
        cout << "-----------------------------\n";
    }
}

// Quick Sort
void quickSort_ssd(Student_ssd* arr_ssd, int first_ssd, int last_ssd, int* swapCount_ssd) {
    if (first_ssd >= last_ssd) return;
    int pi_ssd = partition_ssd(arr_ssd, first_ssd, last_ssd, swapCount_ssd);
    quickSort_ssd(arr_ssd, first_ssd, pi_ssd - 1, swapCount_ssd);
    quickSort_ssd(arr_ssd, pi_ssd + 1, last_ssd, swapCount_ssd);
}

// Partition function
int partition_ssd(Student_ssd* arr_ssd, int first_ssd, int last_ssd, int* swapCount_ssd) {
    Student_ssd pivot_ssd = arr_ssd[last_ssd];
    int i_ssd = first_ssd - 1;
    for (int j_ssd = first_ssd; j_ssd < last_ssd; j_ssd++) {
        if (arr_ssd[j_ssd].roll_no_ssd < pivot_ssd.roll_no_ssd) {
            swap_ssd(&arr_ssd[i_ssd + 1], &arr_ssd[j_ssd], swapCount_ssd);
            i_ssd++;
        }
    }
    swap_ssd(&arr_ssd[i_ssd + 1], &arr_ssd[last_ssd], swapCount_ssd);
    return i_ssd + 1;
}
```

---

## 🧪 Example Output (Sample)

```text
Enter number of students: 8

Original Student Array:
Roll No Name    Total Marks
87      Rohan   54
94      Vikas   56
93      Neha    44
63      Aarav   39
60      Abir    51
41      Neha    5
37      Isha    25
68      Anaya   51

--- Pass 1 ---
Swapping Roll 94 (Vikas) and Roll 93 (Neha)
Swapping Roll 94 (Vikas) and Roll 63 (Aarav)
...
Array after Pass 1:
...

After Bubble Sort (by Roll No):
Roll No Name    Total Marks
37      Isha    25
41      Neha    5
60      Abir    51
63      Aarav   39
68      Anaya   51
87      Rohan   54
93      Neha    44
94      Vikas   56
Number of swaps in Bubble Sort: 22

After Quick Sort (by Roll No):
Roll No Name    Total Marks
37      Isha    25
41      Neha    5
60      Abir    51
63      Aarav   39
68      Anaya   51
87      Rohan   54
93      Neha    44
94      Vikas   56
Number of swaps in Quick Sort: 14
```

*(Note: Actual values and swap counts will vary because data is randomly generated.)*

---

## 🧠 Memory Visualization

### Dynamic Arrays in Heap

Two arrays are allocated:

- `arr1_ssd` → used for **Bubble Sort**  
- `arr2_ssd` → used for **Quick Sort**

| Index | name_ssd | roll_no_ssd | total_marks_ssd |
|-------|----------|-------------|------------------|
| 0     | Rohan    | 87          | 54               |
| 1     | Vikas    | 94          | 56               |
| 2     | Neha     | 93          | 44               |
| 3     | Aarav    | 63          | 39               |
| 4     | Abir     | 60          | 51               |
| 5     | Neha     | 41          | 5                |
| 6     | Isha     | 37          | 25               |
| 7     | Anaya    | 68          | 51               |

- Elements are swapped **in-place** during sorting.  
- **Swap counters** track total number of swaps for each method:
  - `bubbleSwaps_ssd` for Bubble Sort  
  - `quickSwaps_ssd` for Quick Sort  

---

## ✅ Conclusion

This assignment demonstrates:

- Implementation of **Bubble Sort** and **Quick Sort** on a **structure array** using a specific **key (roll number)**.  
- Use of **dynamic memory allocation** to handle variable-sized student lists.  
- How to **count swaps** to compare algorithm efficiency.  

From typical runs:
- **Bubble Sort** performs **more swaps** and is slower for large data.  
- **Quick Sort** performs **fewer swaps** and is more efficient, especially as the number of students increases.

This helps understand **time and swap complexity** differences between a **basic O(n²)** sorting algorithm and an efficient **O(n log n)** algorithm.

