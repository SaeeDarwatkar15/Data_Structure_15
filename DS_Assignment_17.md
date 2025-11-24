# 🧮 Assignment 17: Polynomial Addition Using Singly Linked List

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
Write a C++ program to represent polynomials using a **singly linked list** and perform **addition of two polynomials**, displaying the result in proper algebraic form.

---

## 📝 Problem Statement

Write a C++ program to:

1. Represent each polynomial as a **singly linked list**, where each node stores:
   - Coefficient (`coef_ssd`)
   - Exponent (`exp_ssd`)
2. Insert terms in **descending order of exponents**.
3. Add **two polynomials** term-by-term:
   - If exponents are equal → add coefficients.
   - If exponents differ → copy the term with the higher exponent.
4. Display:
   - First polynomial  
   - Second polynomial  
   - Resultant polynomial after addition  

---

## 📘 Theory

### 🔹 Polynomial Representation Using Linked List

Each node of the list represents one term:  
\[
\text{coef\_ssd} \cdot x^{\text{exp\_ssd}}
\]

Node structure:

- `coef_ssd` → coefficient  
- `exp_ssd` → exponent  
- `next_ssd` → pointer to next term  

Polynomials are stored in **descending order of exponent** for easy addition.

### 🔹 Why Linked List?

- Dynamic number of terms.  
- Easy insertion in sorted order (by exponent).  
- Efficient term-wise traversal for operations like **addition**.

---

### 🔹 Polynomial Addition Logic

Given two polynomials:

\[
P(x) = \sum a_i x^{e_i}
\]
\[
Q(x) = \sum b_j x^{f_j}
\]

We traverse both lists together:

- If `exp1 > exp2` → copy term from first polynomial to result.  
- If `exp1 < exp2` → copy term from second polynomial to result.  
- If `exp1 == exp2` → add coefficients:
  \[
  \text{sum\_coef} = a_i + b_j
  \]
  - If `sum_coef != 0`, insert term with that exponent into result.

After one list finishes, copy any remaining terms from the other list.

---

## 🔍 Algorithm (Step-by-Step)

### 1️⃣ Insert Term in Sorted Order

`insertNode_ssd(head, coef, exp)`:

1. Create new node with given `coef` and `exp`.  
2. If list is empty OR new exponent is **greater** than head’s exponent:
   - Insert at beginning.
3. Otherwise:
   - Traverse till you find correct position where `next.exp <= new.exp`.
   - Insert node there.
4. Return updated head.

---

### 2️⃣ Polynomial Addition

`addPoly_ssd(poly1, poly2)`:

1. Initialize `result = NULL`.  
2. Use pointers `p1` for `poly1` and `p2` for `poly2`.  
3. While both `p1` and `p2` are not NULL:
   - If `p1->exp_ssd > p2->exp_ssd`:
     - Insert `(p1->coef_ssd, p1->exp_ssd)` into `result`.
     - Move `p1` forward.
   - Else if `p1->exp_ssd < p2->exp_ssd`:
     - Insert `(p2->coef_ssd, p2->exp_ssd)` into `result`.
     - Move `p2` forward.
   - Else (same exponent):
     - `sum = p1->coef_ssd + p2->coef_ssd`
     - If `sum != 0`, insert `(sum, exp)` into `result`.
     - Move both `p1` and `p2` forward.
4. If any terms left in `p1` or `p2`, copy remaining terms to `result`.  
5. Return `result` as sum polynomial.

---

## 💻 C++ Program

```cpp
#include <iostream>

using namespace std;

// Node structure for polynomial term
struct Node_ssd {
    int coef_ssd;          // Coefficient
    int exp_ssd;           // Exponent
    Node_ssd* next_ssd;    // Pointer to next term
};

// Create a new node
Node_ssd* createNode_ssd(int coef_ssd, int exp_ssd) {
    Node_ssd* newNode_ssd = new Node_ssd;
    newNode_ssd->coef_ssd = coef_ssd;
    newNode_ssd->exp_ssd = exp_ssd;
    newNode_ssd->next_ssd = nullptr;
    return newNode_ssd;
}

// Insert node in descending order of exponent
Node_ssd* insertNode_ssd(Node_ssd* head_ssd, int coef_ssd, int exp_ssd) {
    Node_ssd* newNode_ssd = createNode_ssd(coef_ssd, exp_ssd);
    if (!head_ssd || head_ssd->exp_ssd < exp_ssd) {
        newNode_ssd->next_ssd = head_ssd;
        return newNode_ssd;
    }
    Node_ssd* temp_ssd = head_ssd;
    while (temp_ssd->next_ssd && temp_ssd->next_ssd->exp_ssd >= exp_ssd)
        temp_ssd = temp_ssd->next_ssd;
    newNode_ssd->next_ssd = temp_ssd->next_ssd;
    temp_ssd->next_ssd = newNode_ssd;
    return head_ssd;
}

// Print polynomial
void printPoly_ssd(Node_ssd* head_ssd) {
    if (!head_ssd) {
        cout << "0\n";
        return;
    }
    Node_ssd* temp_ssd = head_ssd;
    while (temp_ssd) {
        if (temp_ssd->coef_ssd != 0) {
            if (temp_ssd->exp_ssd == 0)
                cout << temp_ssd->coef_ssd;
            else if (temp_ssd->exp_ssd == 1)
                cout << temp_ssd->coef_ssd << "x";
            else
                cout << temp_ssd->coef_ssd << "x^" << temp_ssd->exp_ssd;

            if (temp_ssd->next_ssd && temp_ssd->next_ssd->coef_ssd >= 0)
                cout << " + ";
        }
        temp_ssd = temp_ssd->next_ssd;
    }
    cout << "\n";
}

// Add two polynomials
Node_ssd* addPoly_ssd(Node_ssd* poly1_ssd, Node_ssd* poly2_ssd) {
    Node_ssd* result_ssd = nullptr;
    Node_ssd* p1_ssd = poly1_ssd;
    Node_ssd* p2_ssd = poly2_ssd;

    while (p1_ssd && p2_ssd) {
        if (p1_ssd->exp_ssd > p2_ssd->exp_ssd) {
            result_ssd = insertNode_ssd(result_ssd, p1_ssd->coef_ssd, p1_ssd->exp_ssd);
            p1_ssd = p1_ssd->next_ssd;
        } else if (p1_ssd->exp_ssd < p2_ssd->exp_ssd) {
            result_ssd = insertNode_ssd(result_ssd, p2_ssd->coef_ssd, p2_ssd->exp_ssd);
            p2_ssd = p2_ssd->next_ssd;
        } else {
            int sum_ssd = p1_ssd->coef_ssd + p2_ssd->coef_ssd;
            if (sum_ssd != 0)
                result_ssd = insertNode_ssd(result_ssd, sum_ssd, p1_ssd->exp_ssd);
            p1_ssd = p1_ssd->next_ssd;
            p2_ssd = p2_ssd->next_ssd;
        }
    }

    while (p1_ssd) {
        result_ssd = insertNode_ssd(result_ssd, p1_ssd->coef_ssd, p1_ssd->exp_ssd);
        p1_ssd = p1_ssd->next_ssd;
    }

    while (p2_ssd) {
        result_ssd = insertNode_ssd(result_ssd, p2_ssd->coef_ssd, p2_ssd->exp_ssd);
        p2_ssd = p2_ssd->next_ssd;
    }

    return result_ssd;
}

// Delete polynomial to free memory
void deletePoly_ssd(Node_ssd* head_ssd) {
    while (head_ssd) {
        Node_ssd* temp_ssd = head_ssd;
        head_ssd = head_ssd->next_ssd;
        delete temp_ssd;
    }
}

// Main function
int main() {
    Node_ssd* poly1_ssd = nullptr;
    Node_ssd* poly2_ssd = nullptr;
    int n_ssd, coef_ssd, exp_ssd;

    // Input first polynomial
    cout << "Enter number of terms in first polynomial: ";
    cin >> n_ssd;
    cout << "Enter terms (coefficient and exponent):\n";
    for (int i = 0; i < n_ssd; i++) {
        cin >> coef_ssd >> exp_ssd;
        poly1_ssd = insertNode_ssd(poly1_ssd, coef_ssd, exp_ssd);
    }

    // Input second polynomial
    cout << "Enter number of terms in second polynomial: ";
    cin >> n_ssd;
    cout << "Enter terms (coefficient and exponent):\n";
    for (int i = 0; i < n_ssd; i++) {
        cin >> coef_ssd >> exp_ssd;
        poly2_ssd = insertNode_ssd(poly2_ssd, coef_ssd, exp_ssd);
    }

    // Display polynomials
    cout << "\nFirst Polynomial: ";
    printPoly_ssd(poly1_ssd);

    cout << "Second Polynomial: ";
    printPoly_ssd(poly2_ssd);

    // Add and display sum
    Node_ssd* sum_ssd = addPoly_ssd(poly1_ssd, poly2_ssd);
    cout << "\nSum of Polynomials: ";
    printPoly_ssd(sum_ssd);

    // Free memory
    deletePoly_ssd(poly1_ssd);
    deletePoly_ssd(poly2_ssd);
    deletePoly_ssd(sum_ssd);

    return 0;
}
```

---

## 🧪 Example Execution

```text
Enter number of terms in first polynomial: 3
Enter terms (coefficient and exponent):
5 4
6 3
3 0
Enter number of terms in second polynomial: 4
Enter terms (coefficient and exponent):
8 3
6 2
6 5
9 0

First Polynomial: 5x^4 + 6x^3 + 3
Second Polynomial: 6x^5 + 8x^3 + 6x^2 + 9

Sum of Polynomials: 6x^5 + 5x^4 + 14x^3 + 6x^2 + 12
```

---

## 🧠 Memory Visualization

Each polynomial is a singly linked list like:

```text
First Polynomial (5x^4 + 6x^3 + 3):

[coef=5, exp=4] -> [coef=6, exp=3] -> [coef=3, exp=0] -> NULL
```

Result list after addition:

```text
[6, 5] -> [5, 4] -> [14, 3] -> [6, 2] -> [12, 0] -> NULL
```

Each node:

| Field       | Description          |
|------------|----------------------|
| `coef_ssd` | Coefficient of term  |
| `exp_ssd`  | Exponent of term     |
| `next_ssd` | Pointer to next node |

---

## ✅ Conclusion

This program:

- Represents polynomials using **singly linked lists**.
- Inserts terms in **sorted order** by exponent.
- Adds two polynomials by:
  - Comparing exponents.
  - Combining like terms.
  - Copying unmatched terms.
- Displays all polynomials in clear algebraic format.

This strengthens understanding of **linked lists**, **dynamic structures**, and **polynomial operations** in C++.

