# 🧩 Assignment 8: Quick Sort on Marks & Min–Max Using Divide and Conquer

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program performs **Quick Sort** on marks of `n` students with **pass-by-pass analysis**, and finds **Minimum & Maximum** marks using the **Divide and Conquer** strategy.

---

## 📝 Problem Statement

Write a C++ program to:

1. Input marks of `n` students.
2. Sort the marks in **ascending order** using **Quick Sort** (without built-in functions).
3. Display the **array after each pass** of Quick Sort.
4. Find **Minimum** and **Maximum** marks using the **Divide and Conquer** recursive strategy.

---

## 📘 Theory

### 🔹 Quick Sort  
- A **divide-and-conquer** sorting algorithm.  
- Selects a **pivot**, partitions the array, and recursively sorts both halves.  
- Pass-by-pass tracing helps visualize how elements rearrange after each partition.  
- Time Complexity:  
  - Best / Average → **O(n log n)**  
  - Worst → **O(n²)**  

### 🔹 Divide & Conquer Min–Max  
- Splits array into halves recursively.  
- Finds minimum & maximum in each half.  
- Combines results to get global min & max.  
- More efficient than linear scanning for large datasets.

---

## 📊 Algorithm

### ✅ Quick Sort Algorithm

1. Select **pivot = last element**.  
2. Partition array into:
   - Elements **≤ pivot**
   - Elements **> pivot**
3. Swap elements during partitioning.
4. Place pivot into correct position.
5. Print array **after each partition pass**.
6. Recursively sort left and right subarrays.

---

### ✅ Min–Max Algorithm (Divide & Conquer)

1. If single element → min = max = element.  
2. If two elements → compare directly.  
3. Otherwise:
   - Split array into two halves.
   - Recursively find min+max of each half.
   - Combine results:
     - `overall_min = min(left_min, right_min)`
     - `overall_max = max(left_max, right_max)`

---

## 💻 C++ Program

```cpp
#include <iostream>
using namespace std;

// Function to swap two values
void swap_ssd(int& a_ssd, int& b_ssd) {
    int temp_ssd = a_ssd;
    a_ssd = b_ssd;
    b_ssd = temp_ssd;
}

// Partition function for Quick Sort
int partition_ssd(int* arr_ssd, int low_ssd, int high_ssd) {
    int pivot_ssd = arr_ssd[high_ssd];
    int i_ssd = low_ssd - 1;

    for (int j_ssd = low_ssd; j_ssd < high_ssd; j_ssd++) {
        if (arr_ssd[j_ssd] <= pivot_ssd) {
            i_ssd++;
            swap_ssd(arr_ssd[i_ssd], arr_ssd[j_ssd]);
        }
    }
    swap_ssd(arr_ssd[i_ssd + 1], arr_ssd[high_ssd]);
    return i_ssd + 1;
}

// Quick Sort with pass-by-pass display
void quickSort_ssd(int* arr_ssd, int low_ssd, int high_ssd, int pass_ssd = 1) {
    if (low_ssd < high_ssd) {
        int pi_ssd = partition_ssd(arr_ssd, low_ssd, high_ssd);

        // Display array after each pass
        cout << "Pass " << pass_ssd << ": ";
        for (int i_ssd = 0; i_ssd <= high_ssd; i_ssd++)
            cout << arr_ssd[i_ssd] << " ";
        cout << endl;

        quickSort_ssd(arr_ssd, low_ssd, pi_ssd - 1, pass_ssd + 1);
        quickSort_ssd(arr_ssd, pi_ssd + 1, high_ssd, pass_ssd + 1);
    }
}

// Find minimum and maximum using Divide & Conquer
pair<int, int> minMax_ssd(int* arr_ssd, int low_ssd, int high_ssd) {
    if (low_ssd == high_ssd)
        return {arr_ssd[low_ssd], arr_ssd[low_ssd]};

    if (high_ssd == low_ssd + 1) {
        if (arr_ssd[low_ssd] < arr_ssd[high_ssd])
            return {arr_ssd[low_ssd], arr_ssd[high_ssd]};
        else
            return {arr_ssd[high_ssd], arr_ssd[low_ssd]};
    }

    int mid_ssd = (low_ssd + high_ssd) / 2;

    pair<int, int> left_ssd = minMax_ssd(arr_ssd, low_ssd, mid_ssd);
    pair<int, int> right_ssd = minMax_ssd(arr_ssd, mid_ssd + 1, high_ssd);

    int min_ssd = (left_ssd.first < right_ssd.first) ? left_ssd.first : right_ssd.first;
    int max_ssd = (left_ssd.second > right_ssd.second) ? left_ssd.second : right_ssd.second;

    return {min_ssd, max_ssd};
}

// Display array
void displayArray_ssd(int* arr_ssd, int n_ssd) {
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++)
        cout << arr_ssd[i_ssd] << " ";
    cout << endl;
}

int main() {
    int n_ssd;
    cout << "Enter number of students: ";
    cin >> n_ssd;

    int* marks_ssd = new int[n_ssd];

    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        cout << "Enter marks of student " << i_ssd + 1 << ": ";
        cin >> marks_ssd[i_ssd];
    }

    cout << "\nOriginal Marks: ";
    displayArray_ssd(marks_ssd, n_ssd);
    cout << endl;

    // Quick Sort
    cout << "Sorting Pass-by-Pass:\n";
    quickSort_ssd(marks_ssd, 0, n_ssd - 1);

    cout << "\nSorted Marks in Ascending Order: ";
    displayArray_ssd(marks_ssd, n_ssd);

    // Min-Max
    pair<int, int> result_ssd = minMax_ssd(marks_ssd, 0, n_ssd - 1);
    cout << "\nMinimum Marks: " << result_ssd.first << endl;
    cout << "Maximum Marks: " << result_ssd.second << endl;

    delete[] marks_ssd;
    return 0;
}
```

---

## 🧪 Example Output

```
Enter number of students: 4
Enter marks of student 1: 20
Enter marks of student 2: 90
Enter marks of student 3: 33
Enter marks of student 4: 76

Original Marks: 20 90 33 76 

Sorting Pass-by-Pass:
Pass 1: 20 33 76 90 
Pass 2: 20 33 

Sorted Marks in Ascending Order: 20 33 76 90 

Minimum Marks: 20
Maximum Marks: 90
```

---

## 🧠 Memory Visualization

### Array of Marks (Stored in Heap)

| Index | Value |
|-------|--------|
| 0     | 20     |
| 1     | 90     |
| 2     | 33     |
| 3     | 76     |

### Quick Sort Reordering (Pass-wise)
- **Pass 1:** Partition entire array  
  → `20 33 76 90`  
- **Pass 2:** Partition left subarray  
  → `20 33`

### Divide & Conquer Min–Max  
Recursively splits:

```
[20 | 90 | 33 | 76]
   /           \
[20,90]       [33,76]
```

Final result:
- Minimum = **20**  
- Maximum = **90**

---

## ✅ Conclusion

This assignment demonstrates:

- Implementation of **Quick Sort** with detailed **pass-by-pass** analysis.  
- Use of **Divide & Conquer** to efficiently compute minimum and maximum.  
- Use of **dynamic memory allocation** for flexible array size.  
- Practical understanding of recursive algorithms and array manipulation.

