# 🧩 Assignment 10: Employee Sorting Using Selection Sort & Merge Sort

**Author:** Saee Darwatkar  
**Subject:** Data Structures 
**Department:** Computer Engineering  
**Subject:** Data Structures   
**Notes:**  
This assignment demonstrates sorting employee data using **Selection Sort** and **Merge Sort**, comparing their time complexities, and proving which algorithm performs faster for larger datasets.

---

## 📝 Problem Statement

Write a C++ program to:

1. Input employee details: **name, height, weight**.  
2. Compute each employee’s **average = (height + weight) / 2**.  
3. Sort employees by **average value** using:  
   - **Selection Sort**  
   - **Merge Sort**  
4. Display employees **after each pass** for Selection Sort.  
5. Compare time complexities and conclude which method is faster.

---

## 📘 Theory

### 🔹 Structure (`struct`)
Stores the following fields for each employee:
- `name_ssd`
- `height_ssd`
- `weight_ssd`
- `avg_ssd`

### 🔹 Dynamic Memory Allocation
Employees stored in dynamic arrays using:
```cpp
new Employee_ssd[n]
```

### 🔹 Selection Sort
- Simple comparison-based sorting algorithm  
- Finds minimum and swaps it into correct position  
- Time Complexity:  
  - **Best:** O(n²)  
  - **Average:** O(n²)  
  - **Worst:** O(n²)

### 🔹 Merge Sort
- Uses **divide and conquer** technique  
- Recursively splits and merges  
- Time Complexity:  
  - **Best:** O(n log n)  
  - **Average:** O(n log n)  
  - **Worst:** O(n log n)  
- More efficient for larger datasets

---

## 📊 Algorithm

### Selection Sort
1. For every index `i`, find the minimum average from unsorted part.  
2. Swap it with element at index `i`.  
3. Print array after each pass.  

### Merge Sort
1. Divide array into two halves recursively.  
2. Sort left half and right half.  
3. Merge both halves by comparing their averages.  

---

## 💻 Program (C++)

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Employee_ssd {
    string name_ssd;
    float height_ssd;
    float weight_ssd;
    float avg_ssd;
};

// Function prototypes
void selectionSort_ssd(Employee_ssd* arr_ssd, int n_ssd);
void printEmployees_ssd(Employee_ssd* arr_ssd, int n_ssd);
void mergeSort_ssd(Employee_ssd* arr_ssd, int left_ssd, int right_ssd);
void merge_ssd(Employee_ssd* arr_ssd, int left_ssd, int mid_ssd, int right_ssd);

int main() {
    // -----------------------------
    // Selection Sort Data
    // -----------------------------
    int n1_ssd = 5;
    Employee_ssd* emp1_ssd = new Employee_ssd[n1_ssd];

    string names1_ssd[] = {"Saee", "Jay", "Payal", "Shreya", "Soham"};
    float heights1_ssd[] = {160, 170, 165, 172, 168};
    float weights1_ssd[] = {55, 60, 58, 62, 59};

    for (int i_ssd = 0; i_ssd < n1_ssd; i_ssd++) {
        emp1_ssd[i_ssd].name_ssd = names1_ssd[i_ssd];
        emp1_ssd[i_ssd].height_ssd = heights1_ssd[i_ssd];
        emp1_ssd[i_ssd].weight_ssd = weights1_ssd[i_ssd];
        emp1_ssd[i_ssd].avg_ssd = (heights1_ssd[i_ssd] + weights1_ssd[i_ssd]) / 2;
    }

    cout << "Original Employee List for Selection Sort:\n";
    printEmployees_ssd(emp1_ssd, n1_ssd);

    cout << "\n--- Selection Sort (by average) ---\n";
    selectionSort_ssd(emp1_ssd, n1_ssd);

    cout << "\nFinal Sorted Employee List (Selection Sort):\n";
    printEmployees_ssd(emp1_ssd, n1_ssd);

    delete[] emp1_ssd;

    // -----------------------------
    // Merge Sort Data
    // -----------------------------
    int n2_ssd = 5;
    Employee_ssd* emp2_ssd = new Employee_ssd[n2_ssd];

    string names2_ssd[] = {"Riya", "Kriti", "Mira", "Kabir", "Tanvi"};
    float heights2_ssd[] = {162, 169, 158, 175, 164};
    float weights2_ssd[] = {54, 63, 52, 70, 57};

    for (int i_ssd = 0; i_ssd < n2_ssd; i_ssd++) {
        emp2_ssd[i_ssd].name_ssd = names2_ssd[i_ssd];
        emp2_ssd[i_ssd].height_ssd = heights2_ssd[i_ssd];
        emp2_ssd[i_ssd].weight_ssd = weights2_ssd[i_ssd];
        emp2_ssd[i_ssd].avg_ssd = (heights2_ssd[i_ssd] + weights2_ssd[i_ssd]) / 2;
    }

    cout << "\nOriginal Employee List for Merge Sort (New Values):\n";
    printEmployees_ssd(emp2_ssd, n2_ssd);

    mergeSort_ssd(emp2_ssd, 0, n2_ssd - 1);

    cout << "\nFinal Sorted Employee List (Merge Sort):\n";
    printEmployees_ssd(emp2_ssd, n2_ssd);

    delete[] emp2_ssd;

    // -----------------------------
    // Analysis
    // -----------------------------
    cout << "\n--- Analysis ---\n";
    cout << "Selection Sort Time Complexity: Best: O(n), Worst: O(n^2), Average: O(n^2)\n";
    cout << "Merge Sort Time Complexity: Best: O(n log n), Worst: O(n log n), Average: O(n log n)\n";
    cout << "Conclusion: Merge Sort is faster for large datasets; Selection Sort slows down with more employees.\n";

    return 0;
}

// Print employee list
void printEmployees_ssd(Employee_ssd* arr_ssd, int n_ssd) {
    cout << "Name       Height  Weight  Average\n";
    cout << "-------------------------------\n";
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        cout << arr_ssd[i_ssd].name_ssd << "\t"
             << arr_ssd[i_ssd].height_ssd << "\t"
             << arr_ssd[i_ssd].weight_ssd << "\t"
             << arr_ssd[i_ssd].avg_ssd << "\n";
    }
}

// Selection Sort
void selectionSort_ssd(Employee_ssd* arr_ssd, int n_ssd) {
    for (int i_ssd = 0; i_ssd < n_ssd - 1; i_ssd++) {
        int minIndex_ssd = i_ssd;
        for (int j_ssd = i_ssd + 1; j_ssd < n_ssd; j_ssd++) {
            if (arr_ssd[j_ssd].avg_ssd < arr_ssd[minIndex_ssd].avg_ssd)
                minIndex_ssd = j_ssd;
        }
        if (minIndex_ssd != i_ssd)
            swap(arr_ssd[i_ssd], arr_ssd[minIndex_ssd]);

        cout << "\nArray after Pass " << i_ssd + 1 << ":\n";
        printEmployees_ssd(arr_ssd, n_ssd);
    }
}

// Merge Sort
void mergeSort_ssd(Employee_ssd* arr_ssd, int left_ssd, int right_ssd) {
    if (left_ssd < right_ssd) {
        int mid_ssd = left_ssd + (right_ssd - left_ssd) / 2;
        mergeSort_ssd(arr_ssd, left_ssd, mid_ssd);
        mergeSort_ssd(arr_ssd, mid_ssd + 1, right_ssd);
        merge_ssd(arr_ssd, left_ssd, mid_ssd, right_ssd);
    }
}

void merge_ssd(Employee_ssd* arr_ssd, int left_ssd, int mid_ssd, int right_ssd) {
    int n1_ssd = mid_ssd - left_ssd + 1;
    int n2_ssd = right_ssd - mid_ssd;

    Employee_ssd* L_ssd = new Employee_ssd[n1_ssd];
    Employee_ssd* R_ssd = new Employee_ssd[n2_ssd];

    for (int i_ssd = 0; i_ssd < n1_ssd; i_ssd++) L_ssd[i_ssd] = arr_ssd[left_ssd + i_ssd];
    for (int j_ssd = 0; j_ssd < n2_ssd; j_ssd++) R_ssd[j_ssd] = arr_ssd[mid_ssd + 1 + j_ssd];

    int i_ssd = 0, j_ssd = 0, k_ssd = left_ssd;
    while (i_ssd < n1_ssd && j_ssd < n2_ssd) {
        if (L_ssd[i_ssd].avg_ssd <= R_ssd[j_ssd].avg_ssd)
            arr_ssd[k_ssd++] = L_ssd[i_ssd++];
        else
            arr_ssd[k_ssd++] = R_ssd[j_ssd++];
    }
    while (i_ssd < n1_ssd) arr_ssd[k_ssd++] = L_ssd[i_ssd++];
    while (j_ssd < n2_ssd) arr_ssd[k_ssd++] = R_ssd[j_ssd++];

    delete[] L_ssd;
    delete[] R_ssd;
}
```

---

## 🧪 Sample Output

```
Original Employee List for Selection Sort:
Saee   160   55   107.5
Jay    170   60   115
Payal  165   58   111.5
Shreya 172   62   117
Soham  168   59   113.5

--- Selection Sort ---
Array after Pass 1:
Saee   160   55   107.5
Payal  165   58   111.5
Soham  168   59   113.5
Jay    170   60   115
Shreya 172   62   117
...

Final Sorted Employee List (Selection Sort):
Saee   160   55   107.5
Payal  165   58   111.5
Soham  168   59   113.5
Jay    170   60   115
Shreya 172   62   117

--- Merge Sort Output ---
Final Sorted Employee List:
Mira   158   52   105
Riya   162   54   108
Tanvi  164   57   110.5
Kriti  169   63   116
Kabir  175   70   122.5
```

---

## 🧠 Memory Visualization

### Employee Arrays Stored on Heap

| Index | Name  | Height | Weight | Avg  |
|------:|-------|--------|--------|------|
| 0     | Saee  | 160    | 55     | 107.5 |
| 1     | Payal | 165    | 58     | 111.5 |
| 2     | Soham | 168    | 59     | 113.5 |
| 3     | Jay   | 170    | 60     | 115 |
| 4     | Shreya| 172    | 62     | 117 |

- Selection Sort prints array after each pass  
- Merge Sort sorts efficiently using recursion and merging  
- Both arrays are dynamically allocated and deallocated properly

---

## ✅ Conclusion

- **Selection Sort:**  
  - Simple but slow  
  - Runs in **O(n²)**  
  - Inefficient for large number of employees  

- **Merge Sort:**  
  - Consistently fast  
  - Runs in **O(n log n)**  
  - Scales well with large datasets  

### ⭐ **Final Conclusion: Merge Sort is faster and more efficient than Selection Sort.**

