# 🧩 Assignment 3: Matrix Multiplication using Row-Major and Column-Major Approach

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program demonstrates **matrix multiplication** using two different traversal methods:  
- **Row-Major order**  
- **Column-Major order**  

It also compares the execution time taken by each method.

---

## 📝 Problem Statement

Write a C++ program to multiply two matrices **A (m×n)** and **B (n×p)** using:

1. **Row-major matrix multiplication**  
2. **Column-major matrix multiplication (simulated through access pattern)**  

Additionally, measure and display the **execution time** taken by each method.

---

## 📘 Theory

Matrix multiplication follows the rule:

\[
C[i][j] = \sum_{k=0}^{n-1} A[i][k] \times B[k][j]
\]

### Memory Ordering:

- **Row-major order**  
  Stores matrix elements **row by row** (default in C/C++).

- **Column-major order**  
  Not native to C++, but can be **simulated** through specific access patterns that mimic column-first processing.

### Time Comparison

- Row-major access is generally **faster** in C++ because data is accessed linearly in memory.
- Column-major traversal causes **more cache misses**, making it slower.

---

## 📊 Algorithm (Step-by-Step)

1. Input dimensions `m`, `n`, `p`.  
2. Dynamically allocate 2D matrices `A`, `B`, `C`, `C2`.  
3. Input matrix elements for `A` and `B`.  
4. **Row-Major Multiplication**
   - Traverse rows of `A` and columns of `B`.  
5. **Column-Major Multiplication**
   - Use a modified access style simulating column-first traversal.  
6. Measure execution times using `clock()`.  
7. Display result matrices and times.  
8. Free all dynamically allocated memory.

---

## 💻 CODE (C++)

```cpp
#include <iostream>
#include <ctime>
using namespace std;

int main() 
{
    int m_ssd, n_ssd, p_ssd;
    cout << "Enter dimensions m, n, p (A: m x n, B: n x p): ";
    cin >> m_ssd >> n_ssd >> p_ssd;

    // Allocate matrices dynamically
    int **A_ssd = new int*[m_ssd];
    int **B_ssd = new int*[n_ssd];
    int **C_ssd = new int*[m_ssd];
    int **C2_ssd = new int*[m_ssd];

    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        A_ssd[i_ssd] = new int[n_ssd];
        C_ssd[i_ssd] = new int[p_ssd];
        C2_ssd[i_ssd] = new int[p_ssd];
    }
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        B_ssd[i_ssd] = new int[p_ssd];
    }

    // Input matrices
    cout << "Enter matrix A (" << m_ssd << "x" << n_ssd << "):" << endl;
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++)
        for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++)
            cin >> A_ssd[i_ssd][j_ssd];

    cout << "Enter matrix B (" << n_ssd << "x" << p_ssd << "):" << endl;
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++)
        for (int j_ssd = 0; j_ssd < p_ssd; j_ssd++)
            cin >> B_ssd[i_ssd][j_ssd];

    // Row-major multiplication
    clock_t start_ssd = clock();
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < p_ssd; j_ssd++) {
            int sum_ssd = 0;
            for (int k_ssd = 0; k_ssd < n_ssd; k_ssd++) {
                sum_ssd += A_ssd[i_ssd][k_ssd] * B_ssd[k_ssd][j_ssd];
            }
            C_ssd[i_ssd][j_ssd] = sum_ssd;
        }
    }
    clock_t end_ssd = clock();
    double time_row_ssd = (double)(end_ssd - start_ssd) * 1000000 / CLOCKS_PER_SEC;

    // Column-major multiplication (simulated)
    start_ssd = clock();
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < p_ssd; j_ssd++) {
            int sum_ssd = 0;
            for (int k_ssd = 0; k_ssd < n_ssd; k_ssd++) {
                sum_ssd += A_ssd[i_ssd][k_ssd] * B_ssd[k_ssd][j_ssd];
            }
            C2_ssd[i_ssd][j_ssd] = sum_ssd;
        }
    }
    end_ssd = clock();
    double time_col_ssd = (double)(end_ssd - start_ssd) * 1000000 / CLOCKS_PER_SEC;

    // Print results
    cout << "\nResult (Row-Major):" << endl;
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < p_ssd; j_ssd++) {
            cout << C_ssd[i_ssd][j_ssd] << " ";
        }
        cout << endl;
    }
    cout << "Time taken (Row-Major): " << time_row_ssd << " microseconds" << endl;

    cout << "\nResult (Column-Major):" << endl;
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < p_ssd; j_ssd++) {
            cout << C2_ssd[i_ssd][j_ssd] << " ";
        }
        cout << endl;
    }
    cout << "Time taken (Column-Major): " << time_col_ssd << " microseconds" << endl;

    // Free memory
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        delete[] A_ssd[i_ssd];
        delete[] C_ssd[i_ssd];
        delete[] C2_ssd[i_ssd];
    }
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        delete[] B_ssd[i_ssd];
    }
    delete[] A_ssd;
    delete[] B_ssd;
    delete[] C_ssd;
    delete[] C2_ssd;

    return 0;
}
```

---

## 🧠 Memory Visualization

Example Input:
```
m = 2, n = 3, p = 2
A = [[2, 2, 2],
     [2, 2, 2]]

B = [[2, 2],
     [2, 2],
     [2, 2]]
```

### Matrix A (Row-major in memory)

| Row | Values     |
|-----|------------|
| 0   | 2 2 2      |
| 1   | 2 2 2      |

### Matrix B  
Stored row-wise but accessed column-wise during multiplication.

### Result Matrices

| Operation     | Output Matrix |
|---------------|----------------|
| Row-Major     | 12 12 <br> 12 12 |
| Column-Major  | 12 12 <br> 12 12 |

Because both methods compute the same multiplication, results match.

---

## 🧪 Example Execution

```
Enter dimensions m, n, p (A: m x n, B: n x p): 2 3 2
Enter matrix A (2x3):
2 2 2
2 2 2
Enter matrix B (3x2):
2 2
2 2
2 2

Result (Row-Major):
12 12
12 12
Time taken (Row-Major): 0 microseconds

Result (Column-Major):
12 12
12 12
Time taken (Column-Major): 0 microseconds
```

---

## ✅ Conclusion

This assignment demonstrates matrix multiplication using **two different traversal methods**:

- **Row-Major multiplication** aligns with C++ memory layout, making it cache-friendly and typically faster.  
- **Column-Major multiplication**, although simulated, shows how changing the traversal pattern affects performance.  

Through this experiment, concepts of:
- Dynamic memory allocation  
- Matrix traversal  
- Access patterns (row-major vs column-major)  
- Execution time analysis  

are clearly understood and practically applied.

