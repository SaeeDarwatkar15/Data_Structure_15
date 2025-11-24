# 🧩 Assignment 5: Sparse Matrix Representation and Fast Transpose

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program represents a **sparse matrix** in compact (row, column, value) format and computes its **fast transpose** using counting and indexing arrays.

---

## 📝 Problem Statement

Design a C++ program to:

1. Represent a given **sparse matrix** efficiently using a structure that stores only **non-zero elements** as `(row, col, value)` triples.  
2. Compute its **fast transpose** using a time-efficient algorithm.

**Input:** A matrix of size `m × n` with mostly zero elements.  
**Output:**
- Sparse matrix representation.  
- Fast transpose of the sparse matrix.

---

## 📘 Theory

A **sparse matrix** is a matrix in which most elements are **zero**.  
Instead of storing all `m × n` elements, we store only **non-zero entries** with their positions.

### 1️⃣ Sparse Matrix Representation (Triple Format)

- Each non-zero element is stored as:
  \[
  (row, column, value)
  \]
- First entry (header) stores:
  - Total rows  
  - Total columns  
  - Total non-zero elements  

This reduces memory usage for large, sparse matrices.

### 2️⃣ Fast Transpose of Sparse Matrix

Given a sparse matrix in triple form:

- **Normal Transpose**: swap `(row, col)` for every non-zero element and sort by column.  
- **Fast Transpose**:  
  - Count how many non-zero elements are in each **column**.  
  - Compute **starting index** of each column in the transposed array.  
  - Directly place each element into its correct position in **one pass**.

This makes transpose more efficient than simple scanning.

---

## 🔹 Key Concepts

| Concept                    | Description                                                   |
|----------------------------|---------------------------------------------------------------|
| Sparse Matrix              | Matrix with mostly zero elements                              |
| Triple Representation      | Stores only non-zero elements as `(row, col, value)`         |
| Fast Transpose             | Efficient transpose algorithm using counts and positions      |
| Counting Array `count[]`   | Stores how many elements each column has                      |
| Index Array `index[]`      | Stores starting position for each transposed column          |
| Dynamic Memory Allocation  | Uses `new` and `delete[]` for arrays at runtime               |

---

## 📊 Algorithm (Step-by-Step)

### Input and Sparse Representation

1. **Input** `m_ssd`, `n_ssd` → rows and columns of matrix.  
2. Dynamically allocate a `m_ssd × n_ssd` 2D matrix (`matrix_ssd`).  
3. Input all elements and **count non-zero elements** (`counter_ssd`).  
4. Allocate `sparse_ssd` and `transpose_ssd` arrays of size `counter_ssd + 1`.  
5. Fill **header** of `sparse_ssd`:
   - `sparse_ssd[0].row = m_ssd`  
   - `sparse_ssd[0].col = n_ssd`  
   - `sparse_ssd[0].val = counter_ssd`  
6. Traverse the original matrix:
   - For every non-zero element, store `(row, col, value)` in `sparse_ssd[k]`.

### Fast Transpose Algorithm

7. Set **transpose header**:
   - `transpose_ssd[0].row = sparse_ssd[0].col`  
   - `transpose_ssd[0].col = sparse_ssd[0].row`  
   - `transpose_ssd[0].val = sparse_ssd[0].val`  
8. Allocate `count_ssd[n_ssd]` and initialize to 0.  
9. For each non-zero entry in `sparse_ssd[1..counter_ssd]`:
   - Increment `count_ssd[ col ]`.  
10. Allocate `index_ssd[n_ssd]`.  
11. Compute **starting position** for each column in transpose:
    - `index_ssd[0] = 1`  
    - For `i` from 1 to `n_ssd-1`:
      - `index_ssd[i] = index_ssd[i - 1] + count_ssd[i - 1]`  
12. For each non-zero entry in `sparse_ssd[1..counter_ssd]`:
    - Find column: `col_ssd = sparse_ssd[i].col`  
    - Position in transpose: `pos_ssd = index_ssd[col_ssd]`  
    - Set:
      - `transpose_ssd[pos_ssd].row = sparse_ssd[i].col`  
      - `transpose_ssd[pos_ssd].col = sparse_ssd[i].row`  
      - `transpose_ssd[pos_ssd].val = sparse_ssd[i].val`  
    - Increment `index_ssd[col_ssd]`.  

13. Display both **sparse matrix** and **fast transpose**.  
14. Free all dynamically allocated memory.  

---

## 💻 CODE (C++)

```cpp
#include <iostream>
#include <iomanip>  // for std::setprecision
#include <cstdlib>
using namespace std;

// Structure to store non-zero elements of sparse matrix
struct Sparse_ssd {
    int row, col;
    float val;
};

int main() {
    int m_ssd, n_ssd, counter_ssd = 0;
    float **matrix_ssd = nullptr;
    Sparse_ssd *sparse_ssd = nullptr, *transpose_ssd = nullptr;
    int *count_ssd = nullptr, *index_ssd = nullptr;

    // Step 1: Input matrix size
    cout << "Enter rows and cols: ";
    cin >> m_ssd >> n_ssd;

    // Step 2: Allocate matrix dynamically
    matrix_ssd = new float*[m_ssd];
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++)
        matrix_ssd[i_ssd] = new float[n_ssd];

    // Step 3: Input elements and count non-zero
    cout << "Enter elements:\n";
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++) {
            cin >> matrix_ssd[i_ssd][j_ssd];
            if (matrix_ssd[i_ssd][j_ssd] != 0.0f)
                counter_ssd++;
        }
    }

    // Step 4: Allocate sparse and transpose arrays
    sparse_ssd = new Sparse_ssd[counter_ssd + 1];
    transpose_ssd = new Sparse_ssd[counter_ssd + 1];

    // Fill sparse representation header
    sparse_ssd[0].row = m_ssd;
    sparse_ssd[0].col = n_ssd;
    sparse_ssd[0].val = counter_ssd;

    int k_ssd = 1;
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++) {
            if (matrix_ssd[i_ssd][j_ssd] != 0.0f) {
                sparse_ssd[k_ssd].row = i_ssd;
                sparse_ssd[k_ssd].col = j_ssd;
                sparse_ssd[k_ssd].val = matrix_ssd[i_ssd][j_ssd];
                k_ssd++;
            }
        }
    }

    // Step 5: Display sparse matrix
    cout << "\nSparse Matrix (Row, Col, Value):\n";
    for (int i_ssd = 0; i_ssd <= counter_ssd; i_ssd++)
        cout << sparse_ssd[i_ssd].row << "\t" << sparse_ssd[i_ssd].col << "\t"
             << fixed << setprecision(2) << sparse_ssd[i_ssd].val << endl;

    // Step 6: Fast Transpose
    // 6.0: Header
    transpose_ssd[0].row = sparse_ssd[0].col;  // swap rows and cols
    transpose_ssd[0].col = sparse_ssd[0].row;
    transpose_ssd[0].val = sparse_ssd[0].val;

    // 6.1: Count elements in each column
    count_ssd = new int[n_ssd]();
    for (int i_ssd = 1; i_ssd <= counter_ssd; i_ssd++)
        count_ssd[sparse_ssd[i_ssd].col]++;

    // 6.2: Compute starting index for each column
    index_ssd = new int[n_ssd];
    index_ssd[0] = 1; // first element after header
    for (int i_ssd = 1; i_ssd < n_ssd; i_ssd++)
        index_ssd[i_ssd] = index_ssd[i_ssd - 1] + count_ssd[i_ssd - 1];

    // 6.3: Place elements directly in transpose
    for (int i_ssd = 1; i_ssd <= counter_ssd; i_ssd++) {
        int col_ssd = sparse_ssd[i_ssd].col;
        int pos_ssd = index_ssd[col_ssd];

        transpose_ssd[pos_ssd].row = sparse_ssd[i_ssd].col;
        transpose_ssd[pos_ssd].col = sparse_ssd[i_ssd].row;
        transpose_ssd[pos_ssd].val = sparse_ssd[i_ssd].val;

        index_ssd[col_ssd]++;
    }

    // Step 7: Display fast transpose
    cout << "\nFast Transpose (Row, Col, Value):\n";
    for (int i_ssd = 0; i_ssd <= counter_ssd; i_ssd++)
        cout << transpose_ssd[i_ssd].row << "\t" << transpose_ssd[i_ssd].col << "\t"
             << fixed << setprecision(2) << transpose_ssd[i_ssd].val << endl;

    // Step 8: Free memory
    for (int i_ssd = 0; i_ssd < m_ssd; i_ssd++)
        delete[] matrix_ssd[i_ssd];

    delete[] matrix_ssd;
    delete[] sparse_ssd;
    delete[] transpose_ssd;
    delete[] count_ssd;
    delete[] index_ssd;

    return 0;
}
```

---

## 🧪 Example Execution

**Input:**
```
Enter rows and cols: 3 3
Enter elements:
1 2 0
0 0 8
0 4 0
```

**Output:**
```
Sparse Matrix (Row, Col, Value):
3       3       4.00
0       0       1.00
0       1       2.00
1       2       8.00
2       1       4.00

Fast Transpose (Row, Col, Value):
3       3       4.00
0       0       1.00
1       0       2.00
1       2       4.00
2       1       8.00
```

---

## 🧠 Memory Visualization

### 1️⃣ Original Matrix (3×3)

|   | 0 | 1 | 2 |
|---|---|---|---|
| 0 | 1 | 2 | 0 |
| 1 | 0 | 0 | 8 |
| 2 | 0 | 4 | 0 |

Most of the elements are zero → good case for **sparse storage**.

---

### 2️⃣ Sparse Matrix Representation (Row, Col, Value)

| Row | Col | Value |
|-----|-----|-------|
| 3   | 3   | 4     |
| 0   | 0   | 1     |
| 0   | 1   | 2     |
| 1   | 2   | 8     |
| 2   | 1   | 4     |

- Header: `3 3 4` → 3 rows, 3 columns, 4 non-zero elements.  
- Each next row shows one non-zero value with its position.

---

### 3️⃣ Fast Transpose Representation (Row, Col, Value)

After transposing (swapping row and col):

| Row | Col | Value |
|-----|-----|-------|
| 3   | 3   | 4     |
| 0   | 0   | 1     |
| 1   | 0   | 2     |
| 1   | 2   | 4     |
| 2   | 1   | 8     |

This corresponds to the transposed matrix:

|   | 0 | 1 | 2 |
|---|---|---|---|
| 0 | 1 | 0 | 0 |
| 1 | 2 | 4 | 0 |
| 2 | 0 | 8 | 0 |

---

## ✅ Conclusion

This assignment demonstrates:

- How to **store a sparse matrix efficiently** using triple representation `(row, col, value)`.  
- How to perform a **fast transpose** using:
  - A **count array** to count non-zero elements per column.  
  - An **index array** to compute starting positions in the transposed array.  

This approach is more efficient than a simple transpose for **large sparse matrices**, as it reduces unnecessary scanning and keeps operations closer to **O(nonZero)** time.