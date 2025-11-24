# 🧩 Assignment 24: **Checking Balanced Parentheses Using Stack**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  
This assignment demonstrates how to check if a parentheses expression is **balanced** using the **stack data structure**, an essential concept in expression evaluation and compiler design.

## 📝 Problem Statement
Given a string containing only the characters `(` `)` `{` `}` `[` `]`, determine whether the string is **balanced**.  
A string is considered balanced if:  
1. **Each opening bracket has a matching closing bracket of the same type**  
2. **Order of closing brackets is correct**  
3. **No unmatched brackets remain**

## 📘 Theory
A **stack** follows **LIFO (Last In First Out)**, meaning the last inserted item is removed first. This structure is perfect for tracking opening brackets because nested structures close in reverse order of opening.  
### Why Stack?  
- Opening brackets → **push**  
- Closing brackets → **pop** and compare  
- Mismatch → **Unbalanced**  
- End of string and stack empty → **Balanced**  
### Where It Is Used?  
- **Compilers** (syntax validation)  
- **Expression evaluation**  
- **HTML/XML tag checking**  
- **Balanced tree/graph structures**  

## 🔹 Key Concepts
- **Push:** Insert opening bracket  
- **Pop:** Remove last opening bracket  
- **Top:** Compare with closing bracket  
- **Empty Stack Check:** Detect unmatched closings  
- **Final Stack Check:** Detect unmatched openings  

## 📊 Algorithm
1. Input the parentheses string  
2. Traverse each character  
3. If opening bracket → push  
4. If closing bracket →  
   - If stack empty: return Not Balanced  
   - Pop and compare pair types  
5. After traversal:  
   - Stack empty → Balanced  
   - Not empty → Not Balanced  

## 💻 CODE
```cpp
    #include <iostream>
    #include <stack>
    #include <string>
    using namespace std;

    bool isMatchingPair_ssd(char open_ssd, char close_ssd) {
        if (open_ssd == '(' && close_ssd == ')') return true;
        if (open_ssd == '{' && close_ssd == '}') return true;
        if (open_ssd == '[' && close_ssd == ']') return true;
        return false;
    }

    bool isBalanced_ssd(string exp_ssd) {
        stack<char> st_ssd;
        for (char ch_ssd : exp_ssd) {
            if (ch_ssd == '(' || ch_ssd == '{' || ch_ssd == '[')
                st_ssd.push(ch_ssd);
            else if (ch_ssd == ')' || ch_ssd == '}' || ch_ssd == ']') {
                if (st_ssd.empty()) return false;
                char top_ssd = st_ssd.top();
                st_ssd.pop();
                if (!isMatchingPair_ssd(top_ssd, ch_ssd))
                    return false;
            }
        }
        return st_ssd.empty();
    }

    int main() {
        string exp_ssd;
        cout << "Enter parentheses string: ";
        cin >> exp_ssd;
        if (isBalanced_ssd(exp_ssd))
            cout << "Balanced\n";
        else
            cout << "Not Balanced\n";
        return 0;
    }
```

## 🧮 Example Execution
Input:  
{([])}  
Output:  
Balanced  

Input:  
{}))  
Output:  
Not Balanced  

## 🧠 Memory (Stack) Visualization
Step | Character | Action | Stack  
1 | { | Push | {  
2 | ( | Push | { (  
3 | [ | Push | { ( [  
4 | ] | Pop + Match | { (  
5 | ) | Pop + Match | {  
6 | } | Pop + Match | (empty)  
Final stack empty → **Balanced**

## ✅ Conclusion
This assignment shows how **stack operations** like push, pop, and top help validate balanced parentheses effectively. Using stack logic ensures proper handling of nested and ordered expressions. The concept is vital for compilers, interpreters, and expression evaluators, strengthening the understanding of both **stack data structures** and **algorithmic problem solving**.
