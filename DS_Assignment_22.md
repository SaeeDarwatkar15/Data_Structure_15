# 🧮 Assignment 22: Infix to Postfix Conversion Using Stack (Step-by-Step)

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
Write a C++ program to **convert an infix expression** (e.g. `a-b*c-d/e+f`) into its **postfix form** using a **stack**, and **show all stack operations step by step**.

---

## 📝 Problem Statement

Given an infix expression like:

> `a - b * c - d / e + f`

Convert it into **postfix** notation using a **stack-based algorithm**, while:

- Handling **operators** with different **precedence** (`+, -, *, /, ^`).
- Handling **parentheses** `(` and `)`.
- Printing, for each token:
  - The **current token**
  - The **action performed** (push / pop / append)
  - The **stack content** (bottom → top)
  - The **current postfix output**

Finally, display the **final postfix expression**.

---

## 📘 Theory

### 🔹 Infix vs Postfix

- **Infix:** Operator is **between** operands  
  Example: `a - b * c`
- **Postfix (Reverse Polish Notation):** Operator comes **after** operands  
  Example: `a b c * -`

Postfix advantages:

- No need for parentheses.
- Easy to evaluate using a stack.

---

### 🔹 Role of Stack in Conversion

We use a **stack** to temporarily hold **operators and parentheses**:

- **Operands** (a, b, c, d, …) → directly added to postfix output.
- **Operators** (`+, -, *, /, ^`) → pushed to stack based on **precedence and associativity**.
- **Parentheses**:
  - `'('` → always **pushed**.
  - `')'` → pop until `'('` is found.

---

### 🔹 Operator Precedence & Associativity

| Operator | Precedence | Associativity   |
|----------|------------|-----------------|
| `^`      | Highest    | Right to Left   |
| `*` `/`  | Medium     | Left to Right   |
| `+` `-`  | Lowest     | Left to Right   |

Rules:

- While current operator has **lower or equal** precedence than stack-top (for left-associative), **pop** from stack to output.
- For `^` (right-associative), pop only if stack-top has **higher** precedence.

---

## 🧠 Algorithm (Infix → Postfix)

Let `expr` be the input infix expression.

1. Initialize:
   - Empty **stack** for operators.
   - Empty **string** `output`.
2. Scan the expression **from left to right**, token by token.
3. For each `token`:
   - If `token` is an **operand** (letter/digit):  
     → **Append** it to `output`.
   - If `token` is `'('`:  
     → **Push** it onto the stack.
   - If `token` is `')'`:  
     → Pop from stack and **append to output** until `'('` is found.  
       Remove `'('` from stack.
   - If `token` is an **operator** (`+,-,*,/,^`):
     - While stack is **not empty** and top of stack is **operator**:
       - If current operator is **left-associative** and its **precedence ≤ stack top**, or
       - operator is `^` (right-associative) and **precedence < stack top**  
       → **Pop** stack top to `output`.
     - After popping, **push** current operator to stack.
4. After scanning all tokens:
   - Pop all **remaining operators** from stack to `output`.
5. The final `output` is the **postfix expression**.

---

## 💻 C++ Program (With Step-by-Step Trace)

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <cctype>

#define MAX_SSD 100

// operator stack
char opStack_ssd[MAX_SSD];
int top_ssd = -1;

// push to stack
void push_ssd(char c_ssd) {
    if (top_ssd < MAX_SSD - 1) opStack_ssd[++top_ssd] = c_ssd;
}

// pop from stack
char pop_ssd() {
    if (top_ssd == -1) return '\0';
    return opStack_ssd[top_ssd--];
}

// peek stack top
char peek_ssd() {
    if (top_ssd == -1) return '\0';
    return opStack_ssd[top_ssd];
}

// check if operator
int isOperator_ssd(char c_ssd) {
    return c_ssd=='+' || c_ssd=='-' || c_ssd=='*' || c_ssd=='/' || c_ssd=='^';
}

// precedence (higher number -> higher precedence)
int precedence_ssd(char c_ssd) {
    if (c_ssd == '^') return 3;
    if (c_ssd == '*' || c_ssd == '/') return 2;
    if (c_ssd == '+' || c_ssd == '-') return 1;
    return 0;
}

// safe append single char to C string with space handling
void appendCharToOutput_ssd(char *out_ssd, char token_ssd) {
    if (std::strlen(out_ssd) != 0) std::strcat(out_ssd, " ");
    char tmp_ssd[2] = { token_ssd, '\0' };
    std::strcat(out_ssd, tmp_ssd);
}

// convert current stack to string (bottom -> top)
void stackToString_ssd(char *buf_ssd, int bufsize_ssd) {
    if (top_ssd == -1) {
        std::strncpy(buf_ssd, "(empty)", bufsize_ssd-1);
        buf_ssd[bufsize_ssd-1] = '\0';
        return;
    }
    buf_ssd[0] = '\0';
    for (int i = 0; i <= top_ssd; ++i) {
        char tmp_ssd[4] = {0};
        tmp_ssd[0] = opStack_ssd[i];
        tmp_ssd[1] = '\0';
        std::strncat(buf_ssd, tmp_ssd, bufsize_ssd - std::strlen(buf_ssd) - 1);
        if (i != top_ssd) std::strncat(buf_ssd, " ", bufsize_ssd - std::strlen(buf_ssd) - 1);
    }
}

// main conversion with step printing (aligned)
void infixToPostfixWithSteps_ssd(const char *expr_ssd) {
    char output_ssd[4 * MAX_SSD];
    output_ssd[0] = '\0';
    int len_ssd = std::strlen(expr_ssd);

    std::printf("%-6s %-26s %-20s %s\n", "Token", "Action", "Stack (bottom->top)", "Output");
    std::printf("-------------------------------------------------------------------------------\n");

    for (int i = 0; i < len_ssd; ++i) {
        char token_ssd = expr_ssd[i];
        if (token_ssd == ' ' || token_ssd == '\t') continue;

        char stackBuf_ssd[128];
        if (std::isalnum((unsigned char)token_ssd)) {
            // operand -> append to output
            appendCharToOutput_ssd(output_ssd, token_ssd);
            stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
            std::printf("%-6c %-26s %-20s %s\n", token_ssd, "Operand -> append", stackBuf_ssd, output_ssd);
        }
        else if (token_ssd == '(') {
            push_ssd(token_ssd);
            stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
            std::printf("%-6c %-26s %-20s %s\n", token_ssd, "Push '('", stackBuf_ssd, output_ssd);
        }
        else if (token_ssd == ')') {
            if (peek_ssd() == '\0') {
                stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
                std::printf("%-6c %-26s %-20s %s\n", token_ssd, "')' found, nothing to pop", stackBuf_ssd, output_ssd);
            } else {
                // pop until '('
                while (top_ssd != -1 && peek_ssd() != '(') {
                    char op_ssd = pop_ssd();
                    appendCharToOutput_ssd(output_ssd, op_ssd);
                }
                if (peek_ssd() == '(') pop_ssd(); // remove '('
                stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
                std::printf("%-6c %-26s %-20s %s\n", token_ssd, "Pop until '('", stackBuf_ssd, output_ssd);
            }
        }
        else if (isOperator_ssd(token_ssd)) {
            // pop while top has operator with >= prec (left assoc),
            // but for '^' (right assoc) pop only if strictly greater precedence
            while (top_ssd != -1 && isOperator_ssd(peek_ssd())) {
                if ((token_ssd == '^' && precedence_ssd(peek_ssd()) > precedence_ssd(token_ssd)) ||
                    (token_ssd != '^' && precedence_ssd(peek_ssd()) >= precedence_ssd(token_ssd))) {
                    char op_ssd = pop_ssd();
                    appendCharToOutput_ssd(output_ssd, op_ssd);
                } else break;
            }
            push_ssd(token_ssd);
            stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
            char act_ssd[40];
            std::snprintf(act_ssd, sizeof(act_ssd), "Process operator '%c'", token_ssd);
            std::printf("%-6c %-26s %-20s %s\n", token_ssd, act_ssd, stackBuf_ssd, output_ssd);
        }
        else {
            stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
            std::printf("%-6c %-26s %-20s %s\n", token_ssd, "Ignored/unknown", stackBuf_ssd, output_ssd);
        }
    }

    // pop remaining operators
    while (top_ssd != -1) {
        char op_ssd = pop_ssd();
        if (op_ssd == '(' || op_ssd == ')') continue;
        appendCharToOutput_ssd(output_ssd, op_ssd);
    }
    char stackBuf_ssd[128];
    stackToString_ssd(stackBuf_ssd, sizeof(stackBuf_ssd));
    std::printf("%-6s %-26s %-20s %s\n", "End", "Pop remaining", stackBuf_ssd, output_ssd);

    std::printf("\nFinal Postfix: %s\n", output_ssd);
}

int main() {
    char expr_ssd[MAX_SSD];
    std::printf("Enter infix expression (e.g. a-b*c-d/e+f) : ");
    if (!std::fgets(expr_ssd, sizeof(expr_ssd), stdin)) return 0;
    // remove trailing newline
    size_t ln_ssd = std::strlen(expr_ssd);
    if (ln_ssd > 0 && expr_ssd[ln_ssd-1] == '\n') expr_ssd[ln_ssd-1] = '\0';

    infixToPostfixWithSteps_ssd(expr_ssd);
    return 0;
}
```

---

## 🧪 Example Execution (For `a-b*c-d/e+f`)

```text
Enter infix expression (e.g. a-b*c-d/e+f) : a-b*c-d/e+f
Token  Action                     Stack (bottom->top)  Output
-------------------------------------------------------------------------------
a      Operand -> append          (empty)              a
-      Process operator '-'       -                    a
b      Operand -> append          -                    a b
*      Process operator '*'       - *                  a b
c      Operand -> append          - *                  a b c
-      Process operator '-'       -                    a b c * -
d      Operand -> append          -                    a b c * - d
/      Process operator '/'       - /                  a b c * - d
e      Operand -> append          - /                  a b c * - d e
+      Process operator '+'       +                    a b c * - d e / -
f      Operand -> append          +                    a b c * - d e / - f
End    Pop remaining              (empty)              a b c * - d e / - f +

Final Postfix: a b c * - d e / - f +
```

So, for the infix expression:

> `a - b * c - d / e + f`

The final **postfix expression** is:

> `a b c * - d e / - f +`

(Without spaces: `abc*-de/-f+`)

---

## 🧠 Stack & Output Evolution (Summary)

For `a-b*c-d/e+f`:

| Step | Token | Action                         | Stack        | Output        |
|------|-------|--------------------------------|--------------|---------------|
| 1    | a     | Append operand                 | —            | a             |
| 2    | -     | Push `-`                       | -            | a             |
| 3    | b     | Append operand                 | -            | a b           |
| 4    | *     | Push `*` (higher prec)         | - *          | a b           |
| 5    | c     | Append operand                 | - *          | a b c         |
| 6    | -     | Pop `*`, then pop `-`, push `-`| -            | a b c * -     |
| 7    | d     | Append operand                 | -            | a b c * - d   |
| 8    | /     | Push `/`                       | - /          | a b c * - d   |
| 9    | e     | Append operand                 | - /          | a b c * - d e |
| 10   | +     | Pop `/`, pop `-`, push `+`     | +            | a b c * - d e / - |
| 11   | f     | Append operand                 | +            | a b c * - d e / - f |
| 12   | End   | Pop `+`                        | —            | a b c * - d e / - f + |

---

## ✅ Conclusion

In this assignment, you:

- Implemented **infix to postfix conversion** using a **stack-based algorithm**.
- Handled **operator precedence**, **associativity**, and **parentheses**.
- Printed all **intermediate steps**: token, action, stack contents, and current output.
- Verified that the infix expression `a-b*c-d/e+f` correctly converts to postfix `abc*-de/-f+`.

This example clearly shows how stacks simplify expression conversion and form the base for **expression evaluation** in compilers and calculators.
