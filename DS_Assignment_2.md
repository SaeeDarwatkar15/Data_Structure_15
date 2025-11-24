# 🧩 Assignment 2: Magic Square of Order *n*

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program constructs and verifies **magic squares** of order **n** for:
- **Odd order (n % 2 == 1)** using the **Siamese method**  
- **Doubly even order (n % 4 == 0)** using the **complement method**  
- **Singly even order (n % 2 == 0 && n % 4 != 0)** is **not supported** and shows a message.

---

## 📝 Problem Statement

Write a C++ program to construct and display a **Magic Square** of order **n** such that:

- The sum of all **rows**, **columns**, and both **diagonals** is the same (called the **Magic Constant**).  
- Handle different types of `n`:
  - **Odd order** (3, 5, 7, …) using the **Siamese method**  
  - **Doubly even order** (4, 8, 12, … where `n % 4 == 0`) using the **complement method**  
  - **Singly even order** (6, 10, 14, …) → **display not supported message**

---

## 📘 Theory

A **Magic Square** of order **n** is an `n × n` matrix filled with numbers from **1** to **n²** such that:

- Sum of each **row** is the same  
- Sum of each **column** is the same  
- Sum of both **main diagonals** is the same  

This common value is called the **Magic Constant (M)** and is given by:

\[
M = \frac{n(n^2 + 1)}{2}
\]

### Types of Magic Squares (by order *n*)

1. **Odd Order Magic Square (n = 3, 5, 7, …)**  
   - Constructed using the **Siamese method** (also called the De la Loubère method).  

2. **Doubly Even Magic Square (n = 4, 8, 12, …)**  
   - `n` is divisible by 4 (`n % 4 == 0`).  
   - Constructed using the **complement replacement method**.

3. **Singly Even Magic Square (n = 6, 10, 14, …)**  
   - `n` is even but not divisible by 4.  
   - **More complex algorithm** → in this program, **not implemented**.

---

## 🔹 Key Concepts

| Concept              | Description                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| Magic Square         | Square matrix where all rows, columns, and diagonals have equal sum        |
| Magic Constant (M)   | Common sum of each row/column/diagonal = `n × (n² + 1) / 2`                 |
| Siamese Method       | Method to construct **odd order** magic squares                            |
| Complement Method    | Used for **doubly even** magic squares (`n % 4 == 0`)                       |
| Dynamic Allocation   | Matrix is allocated at runtime using `new` and freed using `delete[]`       |

---

## 📊 Algorithm (Step-by-Step)

1. **Input:** Read `n_ssd` (order of magic square) from the user.  
2. If `n_ssd <= 0` → print **error message** and terminate.  
3. Dynamically allocate a **2D array** `magic_square_ssd[n_ssd][n_ssd]`.  
4. Initialize all elements to **0**.  

### Case 1: Odd Order (n % 2 == 1) – Siamese Method

5. Set starting position:
   - `row_ssd = 0` (first row)  
   - `col_ssd = n_ssd / 2` (middle column)  
6. For each number `num_ssd` from **1** to **n_ssd × n_ssd**:
   - Place `num_ssd` at `magic_square_ssd[row_ssd][col_ssd]`.  
   - Compute next position:
     - `next_row_ssd = row_ssd - 1` (move up)  
     - `next_col_ssd = col_ssd + 1` (move right)  
   - If out of bounds:
     - If `next_row_ssd < 0` → wrap to `n_ssd - 1`  
     - If `next_col_ssd == n_ssd` → wrap to `0`  
   - If next cell is already filled:
     - Move **down** from previous cell:  
       `next_row_ssd = (row_ssd + 1) % n_ssd`  
       `next_col_ssd = col_ssd`  
   - Update `row_ssd = next_row_ssd`, `col_ssd = next_col_ssd`.

### Case 2: Doubly Even Order (n % 4 == 0) – Complement Method

7. Fill the matrix sequentially with numbers from **1** to **n_ssd × n_ssd**.  
8. For each position `(i_ssd, j_ssd)`:
   - If `(i_ssd % 4 == j_ssd % 4)` **OR** `(i_ssd % 4 + j_ssd % 4 == 3)`:
     - Replace the value with its **complement**:  
       `magic_square_ssd[i_ssd][j_ssd] = n_ssd * n_ssd + 1 - magic_square_ssd[i_ssd][j_ssd]`.

### Case 3: Singly Even (n % 2 == 0 && n % 4 != 0)

9. Display message:  
   `"Singly even order (like 6, 10, 14...) is not supported."`  
   Free memory and exit.

---

10. **Output:**
    - Print the constructed magic square.  
    - Print the **Magic Sum** using formula: `n_ssd * (n_ssd * n_ssd + 1) / 2`.  

11. **Free Memory:**  
    - Delete all dynamically allocated rows and the main pointer array.

---

## 💻 CODE (C++)

```cpp
#include <iostream>
using namespace std;

int main() {
    int n_ssd;
    cout << "Enter the value of n : ";
    cin >> n_ssd;

    if (n_ssd <= 0) {
        cout << "Error: n must be positive." << endl;
        return -1;
    }

    // Allocate dynamic 2D array
    int **magic_square_ssd = new int*[n_ssd];
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        magic_square_ssd[i_ssd] = new int[n_ssd];
    }

    // Initialize all cells to 0
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++)
        for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++)
            magic_square_ssd[i_ssd][j_ssd] = 0;

    int row_ssd, col_ssd, next_row_ssd, next_col_ssd, num_ssd;

    // Case 1: Odd n → Siamese method
    if (n_ssd % 2 == 1) {
        row_ssd = 0;
        col_ssd = n_ssd / 2;

        for (num_ssd = 1; num_ssd <= n_ssd * n_ssd; num_ssd++) {
            magic_square_ssd[row_ssd][col_ssd] = num_ssd;
            next_row_ssd = row_ssd - 1;
            next_col_ssd = col_ssd + 1;

            if (next_row_ssd < 0) next_row_ssd = n_ssd - 1;
            if (next_col_ssd == n_ssd) next_col_ssd = 0;

            if (magic_square_ssd[next_row_ssd][next_col_ssd] != 0) {
                next_row_ssd = (row_ssd + 1) % n_ssd;
                next_col_ssd = col_ssd;
            }

            row_ssd = next_row_ssd;
            col_ssd = next_col_ssd;
        }
    }
    // Case 2: Doubly Even (n % 4 == 0)
    else if (n_ssd % 4 == 0) {
        int count_ssd = 1;
        for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
            for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++) {
                magic_square_ssd[i_ssd][j_ssd] = count_ssd;
                count_ssd++;
            }
        }

        // Replace with complements at special positions
        for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
            for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++) {
                if ((i_ssd % 4 == j_ssd % 4) || ((i_ssd % 4 + j_ssd % 4) == 3)) {
                    magic_square_ssd[i_ssd][j_ssd] =
                        n_ssd * n_ssd + 1 - magic_square_ssd[i_ssd][j_ssd];
                }
            }
        }
    }
    // Case 3: Singly Even (not handled)
    else {
        cout << "Singly even order (like 6, 10, 14...) is not supported." << endl;
        for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) delete[] magic_square_ssd[i_ssd];
        delete[] magic_square_ssd;
        return 0;
    }

    // Print the magic square
    cout << "\nMagic Square of order " << n_ssd << ":\n";
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < n_ssd; j_ssd++)
            cout << magic_square_ssd[i_ssd][j_ssd] << "\t";
        cout << endl;
    }

    // Print the magic constant
    cout << "Magic Sum : " << n_ssd * (n_ssd * n_ssd + 1) / 2 << endl;

    // Free memory
    for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) delete[] magic_square_ssd[i_ssd];
    delete[] magic_square_ssd;

    return 0;
}
```

---

## 🧪 Example Execution

### ✅ Case 1: Odd Order (n = 5)

```
Enter the value of n : 5

Magic Square of order 5:
17      24      1       8       15
23      5       7       14      16
4       6       13      20      22
10      12      19      21      3
11      18      25      2       9
Magic Sum : 65
```

### ✅ Case 2: Doubly Even Order (n = 8)

```
Enter the value of n : 8

Magic Square of order 8:
64      2       3       61      60      6       7       57
9       55      54      12      13      51      50      16
17      47      46      20      21      43      42      24
40      26      27      37      36      30      31      33
32      34      35      29      28      38      39      25
41      23      22      44      45      19      18      48
49      15      14      52      53      11      10      56
8       58      59      5       4       62      63      1
Magic Sum : 260
```

### ⚠️ Case 3: Singly Even (n = 6, 10, …)

```
Enter the value of n : 6
Singly even order (like 6, 10, 14...) is not supported.
```

---

## 🧠 Memory / Logic Visualization (Odd Case, n = 3)

For **n = 3**, the Siamese method places numbers as follows:

| Step | Number | Row | Col | Description                            |
|------|--------|-----|-----|----------------------------------------|
| 1    | 1      | 0   | 1   | Start at first row, middle column      |
| 2    | 2      | 2   | 2   | Move up-right, wrap row from -1 to 2   |
| 3    | 3      | 1   | 0   | Move up-right, wrap col from 3 to 0    |
| 4    | 4      | 2   | 0   | Next up-right is filled → move down    |
| 5    | 5      | 0   | 2   | Move up-right with wrap                |
| 6    | 6      | 1   | 2   | Move up-right                          |
| 7    | 7      | 2   | 1   | Move up-right                          |
| 8    | 8      | 0   | 0   | Move up-right with wrap                |
| 9    | 9      | 1   | 1   | Move up-right                          |

**Resulting Magic Square (n = 3):**

```
8   1   6
3   5   7
4   9   2
```

- Row sums: `8 + 1 + 6 = 15`, etc.  
- Column sums: `8 + 3 + 4 = 15`, etc.  
- Diagonal sums: `8 + 5 + 2 = 15`, `6 + 5 + 4 = 15`.  

---

## ✅ Conclusion

This assignment successfully demonstrates the construction of **magic squares** using different techniques based on the order **n**:

- For **odd order**, the **Siamese method** is used to place numbers in a wrapped up-right pattern with a special rule when a cell is already filled.  
- For **doubly even order (n % 4 == 0)**, the **complement replacement method** is used to generate a valid magic square.  
- **Singly even orders** are identified and clearly reported as **not supported** in this implementation.

Through this program, concepts of:
- **2D dynamic arrays**  
- **Modular arithmetic**  
- **Matrix traversal**  
- **Mathematical properties of magic squares**

are understood and applied in a clear and structured way.

