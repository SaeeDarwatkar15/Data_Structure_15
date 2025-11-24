# 🧩 Assignment 4: Sparse Matrix and Its Transpose

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program represents a **sparse matrix** in **(row, column, value)** format and finds its **transpose** using a simple scanning method.

---

## 📝 Problem Statement  

Write a program to:

1. Represent a given matrix in **sparse form** using a list of **(row, column, value)** triples.  
2. Compute and display the **transpose** of this sparse matrix.  

The program should:
- Accept a dense matrix as input.  
- Convert it into a **sparse representation** (only non-zero elements).  
- Compute its **transpose** by swapping row and column indices and scanning column-wise.  
- Display both the **sparse matrix** and its **transpose**.  

---

## 📘 Theory

A **sparse matrix** is a matrix in which most of the elements are **zero**.  
Instead of storing all `m × n` elements, we store only the **non-zero entries** along with their positions.

### Sparse Matrix Representation (Triple Form)

- Each non-zero element is stored as a **triple**:  
  `(row_index, column_index, value)`
- First entry usually stores:  
  `(total_rows, total_cols, non_zero_count)`

This saves memory and makes operations more efficient when matrices are large but sparse.

### Transpose of a Sparse Matrix

- The transpose of a matrix **A** is a matrix **Aᵀ** obtained by **swapping rows and columns**:
  \[
  A(i, j) \rightarrow A^T(j, i)
  \]
- In sparse representation, this means:
  - Each triple `(row, col, val)` becomes `(col, row, val)`.
  - For a **sorted** representation, we usually output entries **column-wise**.

---

## 🔹 Key Concepts

| Concept                  | Description                                                   |
|--------------------------|---------------------------------------------------------------|
| Sparse Matrix            | Matrix with mostly zero values                                |
| Triple Representation    | Stores only non-zero values as (row, col, value)             |
| Header Entry             | First triple storing (rows, cols, nonZeroCount)              |
| Transpose                | Swaps row and column indices of each element                 |
| Dynamic Memory Allocation| Uses `new` and `delete[]` for runtime memory management      |

---

## 📊 Algorithm (Step-by-Step)

1. **Start the program.**  
2. Input the number of rows `m` and columns `n` of the matrix.  
3. Dynamically allocate memory for a `m × n` matrix.  
4. Read all matrix elements and **count non-zero elements**.  
5. Allocate an array of `Sparse` structures of size `nonZeroCount + 1`.  
6. Store **sparse representation** as:
   - `sparse[0] = (m, n, nonZeroCount)`  
   - For each non-zero element: store `(row_index, col_index, value)` in next entries.  
7. Display the sparse matrix (row, col, value).  
8. Allocate another `Sparse` array `transpose` of same size.  
9. Set header of transpose:
   - `transpose[0].row = sparse[0].col`  
   - `transpose[0].col = sparse[0].row`  
   - `transpose[0].val = sparse[0].val`  
10. For each column `j` from `0` to `n - 1`:
    - Scan all non-zero entries in `sparse[1..nonZeroCount]`.  
    - If `sparse[i].col == j`, then in `transpose`:
      - `row = sparse[i].col`  
      - `col = sparse[i].row`  
      - `val = sparse[i].val`  
11. Display the transpose in sparse format.  
12. Free all dynamically allocated memory.  
13. **End program.**

---

## 💻 CODE (C++)

```cpp
#include <iostream>
using namespace std;

// Structure for sparse matrix representation
struct Sparse {
    int row, col;
    float val;
};

int main() {
    int m, n; // rows and cols
    float** matrix = 0;
    Sparse* sparse = 0;
    Sparse* transpose = 0;

    // Input rows and columns
    cout << "Enter the no of rows: ";
    cin >> m;
    cout << "Enter the no of cols: ";
    cin >> n;

    // Dynamic allocation of matrix
    matrix = new float*[m];
    for (int i = 0; i < m; i++)
        matrix[i] = new float[n];

    // Count non-zero elements
    int counter = 0;
    cout << "Enter the elements in the Matrix:\n";
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cin >> matrix[i][j];
            if (matrix[i][j] != 0.0f)
                counter++;
        }
    }

    // Print full matrix
    cout << "\nThe Matrix is:\n";
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++)
            cout << matrix[i][j] << "\t";
        cout << endl;
    }

    // Allocate sparse representation
    sparse = new Sparse[counter + 1];

    // Header stores dimensions and non-zero count
    sparse[0].row = m;
    sparse[0].col = n;
    sparse[0].val = static_cast<float>(counter);

    // Store non-zero values
    int k = 1;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] != 0.0f) {
                sparse[k].row = i;
                sparse[k].col = j;
                sparse[k].val = matrix[i][j];
                k++;
            }
        }
    }

    // Display sparse matrix
    cout << "\nSparse Matrix Representation (Row, Col, Value):\n";
    for (int i = 0; i <= counter; i++)
        cout << sparse[i].row << "\t" << sparse[i].col << "\t" << sparse[i].val << endl;

    // --- Simple Transpose ---
    transpose = new Sparse[counter + 1];
    transpose[0].row = sparse[0].col;  // rows become cols
    transpose[0].col = sparse[0].row;  // cols become rows
    transpose[0].val = sparse[0].val;  // non-zero count stays same

    k = 1;
    for (int j = 0; j < n; j++) {  // column-wise scanning
        for (int i = 1; i <= counter; i++) {
            if (sparse[i].col == j) {
                transpose[k].row = sparse[i].col;
                transpose[k].col = sparse[i].row;
                transpose[k].val = sparse[i].val;
                k++;
            }
        }
    }

    // Display transpose
    cout << "\nTranspose of Sparse Matrix (Row, Col, Value):\n";
    for (int i = 0; i <= counter; i++)
        cout << transpose[i].row << "\t" << transpose[i].col << "\t" << transpose[i].val << endl;

    // Free memory
    for (int i = 0; i < m; i++)
        delete[] matrix[i];
    delete[] matrix;
    delete[] sparse;
    delete[] transpose;

    return 0;
}
```

---

## 🧪 Example Execution

**Input**
```
Enter the no of rows: 3
Enter the no of cols: 3
Enter the elements in the Matrix:
1 0 0
1 0 0
0 0 4
```

**Output**
```
The Matrix is:
1       0       0
1       0       0
0       0       4

Sparse Matrix Representation (Row, Col, Value):
3       3       3
0       0       1
1       0       1
2       2       4

Transpose of Sparse Matrix (Row, Col, Value):
3       3       3
0       0       1
0       1       1
2       2       4
```

---

## 🧠 Memory Visualization

### 1️⃣ Full Matrix (3×3)

|      | Col0 | Col1 | Col2 |
| ---- | ---- | ---- | ---- |
| Row0 | 1    | 0    | 0    |
| Row1 | 1    | 0    | 0    |
| Row2 | 0    | 0    | 4    |

Most elements are zero → good candidate for **sparse representation**.

---

### 2️⃣ Sparse Representation (Triple Form)

Each non-zero value is stored as **(row, col, value)**.  
First entry stores **(rows, cols, nonZeroCount)**.

| Index | Row | Col | Value |
|-------|-----|-----|-------|
| 0     | 3   | 3   | 3     |
| 1     | 0   | 0   | 1     |
| 2     | 1   | 0   | 1     |
| 3     | 2   | 2   | 4     |

---

### 3️⃣ Transpose Sparse Representation

Row and column indices are swapped.

| Index | Row | Col | Value |
|-------|-----|-----|-------|
| 0     | 3   | 3   | 3     |
| 1     | 0   | 0   | 1     |
| 2     | 0   | 1   | 1     |
| 3     | 2   | 2   | 4     |

This corresponds to the transpose matrix:

|      | Col0 | Col1 | Col2 |
| ---- | ---- | ---- | ---- |
| Row0 | 1    | 1    | 0    |
| Row1 | 0    | 0    | 0    |
| Row2 | 0    | 0    | 4    |

---

## ✅ Conclusion

This assignment demonstrates:

- How to **convert a dense matrix into sparse (triple) form** by storing only non-zero elements.  
- How to compute the **transpose** of a sparse matrix efficiently by:
  - Swapping row and column indices.
  - Scanning the original sparse representation column-wise.

It reinforces key concepts of:
- **Dynamic memory allocation** (`new`, `delete[]`)  
- **Efficient storage** of sparse data  
- **Matrix transpose logic** using sparse representation  

This method is especially useful when working with **large matrices** containing mostly zeros, saving both **memory** and **processing time**.