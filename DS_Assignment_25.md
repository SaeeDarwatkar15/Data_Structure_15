# 🧩 Assignment 25: **Evaluation of Postfix Expression Using Stack**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  

This assignment explains how to **evaluate a postfix expression (Reverse Polish Notation)** using the **stack data structure**. The expression contains **single-digit operands** and **binary operators** `+`, `-`, `*`, `/`.

## 📝 Problem Statement
You are given a **postfix expression** (also called **Reverse Polish Notation**) consisting of:
- **Operands:** single-digit numbers `0–9`  
- **Operators:** `+`, `-`, `*`, `/` (binary operators)  

Your task is to:
- **Evaluate** the postfix expression using a **stack**  
- **Return / display** the final numeric result  

Example:  
Postfix expression: `23+5*`  
Evaluation: `(2 + 3) * 5 = 25`

## 📘 Theory

### 🔹 What is Postfix Expression?
- In **infix** notation: operator is **between** operands → `2 + 3`  
- In **postfix** notation: operator is **after** operands → `23+`  

In postfix:
- No need for **parentheses**  
- Evaluation order is **unambiguous**  
- Very suitable for **stack-based evaluation** and **compiler design**

### 🔹 Why Use Stack for Postfix Evaluation?
The evaluation rule is:

1. **Scan expression from left to right**
2. If token is an **operand** → **push** it onto the stack  
3. If token is an **operator** →  
   - **Pop** two operands from the stack  
   - Apply the operator → `lhs (operator) rhs`  
   - **Push the result** back on the stack  

At the end:
- Stack should contain **exactly one element** → **final result**  

If:
- Less than 2 operands are present when an operator appears → **Invalid postfix**  
- Stack has more than 1 element at the end → **Invalid postfix**

### 🔹 Operator Handling
For an operator `op`:
- `rhs_ssd` = top element (popped first)  
- `lhs_ssd` = next top element (popped second)  
- Result = `lhs_ssd op rhs_ssd`  

Example for `23+`:
- Read `2` → push `2`  
- Read `3` → push `3`  
- Read `+` → pop `3` (rhs), pop `2` (lhs) → compute `2 + 3 = 5` → push `5`  

### 🔹 Error Handling and Integer Division
- If division and `rhs_ssd == 0` → **division by zero error**  
- Here `/` is treated as **integer division**  
- Non-digit and non-operator character → **invalid character**  

### 🔹 Applications
- **Expression evaluation** in compilers  
- **Intermediate code** generation  
- **Stack-based virtual machines** (like JVM, interpreters)  

---

## 📊 Algorithm

1. Initialize an empty stack of integers.  
2. For each character `ch_ssd` in the expression:
   - If it is a **space/tab** → ignore  
   - If it is an **operand (digit)**:  
     - Convert char to int → `val_ssd = ch_ssd - '0'`  
     - **Push** `val_ssd` on the stack  
   - If it is an **operator** (`+`, `-`, `*`, `/`):  
     - If stack size < 2 → **invalid expression**  
     - Pop `rhs_ssd` from stack  
     - Pop `lhs_ssd` from stack  
     - Check if operator is `/` and `rhs_ssd == 0` → **error: division by zero**  
     - Compute `res_ssd = lhs_ssd op rhs_ssd`  
     - **Push** `res_ssd` back to stack  
   - Otherwise → invalid character → error  
3. After processing full expression:
   - If stack size != 1 → invalid postfix expression  
   - Else, top of stack is **final result**  

Time Complexity: **O(n)**  
Space Complexity: **O(n)** in worst case  

---

## 💻 CODE

```cpp
    #include <iostream>
    #include <stack>
    #include <string>
    #include <cctype>
    using namespace std;

    int applyOperator_ssd(int lhs_ssd, int rhs_ssd, char op_ssd) {
        switch (op_ssd) {
            case '+': return lhs_ssd + rhs_ssd;
            case '-': return lhs_ssd - rhs_ssd;
            case '*': return lhs_ssd * rhs_ssd;
            case '/': return lhs_ssd / rhs_ssd;  // integer division
        }
        return 0;
    }

    bool evaluatePostfix_ssd(const string &expr_ssd, int &result_ssd) {
        stack<int> st_ssd;

        cout << "\n--- Step-by-step Evaluation ---\n";

        for (char ch_ssd : expr_ssd) {
            if (ch_ssd == ' ' || ch_ssd == '\t') continue;

            if (isdigit((unsigned char)ch_ssd)) {
                int val_ssd = ch_ssd - '0';
                st_ssd.push(val_ssd);
                cout << "Read " << val_ssd << " → push to stack\n";
            }
            else if (ch_ssd == '+' || ch_ssd == '-' || ch_ssd == '*' || ch_ssd == '/') {

                if (st_ssd.size() < 2) return false;

                int rhs_ssd = st_ssd.top(); st_ssd.pop();
                int lhs_ssd = st_ssd.top(); st_ssd.pop();

                cout << "Operator '" << ch_ssd << "' → apply "
                     << lhs_ssd << " " << ch_ssd << " " << rhs_ssd;

                if (ch_ssd == '/' && rhs_ssd == 0) {
                    cout << "\nError: Division by zero.\n";
                    return false;
                }

                int res_ssd = applyOperator_ssd(lhs_ssd, rhs_ssd, ch_ssd);
                st_ssd.push(res_ssd);

                cout << " = " << res_ssd << " → push result\n";
            }
            else {
                cout << "Invalid character: " << ch_ssd << "\n";
                return false;
            }
        }

        if (st_ssd.size() != 1) return false;

        result_ssd = st_ssd.top();
        return true;
    }

    int main() {
        string expr_ssd;
        cout << "Enter postfix expression: ";
        cin >> expr_ssd;

        int result_ssd = 0;

        bool ok_ssd = evaluatePostfix_ssd(expr_ssd, result_ssd);

        if (ok_ssd) {
            cout << "\nFinal Result = " << result_ssd << "\n";
        } else {
            cout << "\nInvalid postfix expression or error.\n";
        }

        return 0;
    }
```


## 🧮 Example Execution

**Input**
    Enter postfix expression: 23+5*

**Step-by-step evaluation**
    Read 2 → push to stack  
    Read 3 → push to stack  
    Operator '+' → apply 2 + 3 = 5 → push result  
    Read 5 → push to stack  
    Operator '*' → apply 5 * 5 = 25 → push result  

**Output**
    Final Result = 25  

---

## 🧠 Stack (Memory) Visualization for `23+5*`

| Step | Symbol | Action                       | Stack Content      |
|------|--------|------------------------------|--------------------|
| 1    | `2`    | Read operand → push          | 2                  |
| 2    | `3`    | Read operand → push          | 2, 3               |
| 3    | `+`    | Pop 3, 2 → compute 2 + 3 = 5 | 5                  |
| 4    | `5`    | Read operand → push          | 5, 5               |
| 5    | `*`    | Pop 5, 5 → compute 5 * 5 =25 | 25                 |

Final stack content → `25` → **Result = 25**

---

## ✅ Conclusion

This assignment shows how **postfix expression evaluation** can be efficiently implemented using a **stack**.  

By scanning the expression from **left to right**, pushing **operands** and applying **operators** using **pop operations**, we learn:

- Practical use of **stack data structure**  
- Difference between **infix** and **postfix** notation  
- How **expression evaluation** works in compilers and interpreters  
- Safe handling of errors like **invalid expressions** and **division by zero**  

This strengthens understanding of both **stack operations** and **expression evaluation algorithms**, which are core topics in **Data Structures and Algorithms**.
