# 🧮 Assignment 21: Stock Price Tracker using Stack (Linked List)

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
Write a C++ program to implement a **stock price tracker** using a **stack implemented with a linked list**.  
The stack should support:
1. `record(price)` – Add a new stock price (push).  
2. `remove()` – Remove and return the most recent price (pop).  
3. `latest()` – Return the most recent price without removing it (peek).  
4. `isEmpty()` – Check if there are no prices recorded.  

---

## 📝 Problem Statement

Design and implement a **simple stock price tracker** that:

- Stores a history of **daily stock prices** as **integers**.  
- Uses a **stack data structure** implemented with a **singly linked list**.  
- Allows the user to:
  - Record a new price.
  - Remove the most recent price.
  - View the latest price.
  - Check whether the stack is empty.

The program should be **menu-driven** and keep running until the user chooses to exit.

---

## 📘 Theory

### 🔹 Stack Data Structure

A **Stack** is a linear data structure that follows the **LIFO (Last In, First Out)** principle:

- **Last inserted element** is the **first one removed**.
- Basic operations:
  - **Push** → Insert element at the top.
  - **Pop** → Remove element from the top.
  - **Peek/Top** → View the top element without removing it.
  - **isEmpty** → Check if stack has any elements.

In this assignment:

- Each stock price is stored as a **node in a linked list**.
- The **top of the stack** points to the **most recent price**.

---

### 🔹 Stack Using Linked List

- Each node has:
  - `price_ssd` → Stock price (int).
  - `next_ssd` → Pointer to the next node.
- `top_ssd` (global pointer) always points to the **top node**.

Advantages:

- No fixed size (dynamic memory).
- Easy push and pop at the beginning of list.

---

## 🧠 Algorithm

### 1. `isEmpty_ssd()`

**Input:** `top_ssd`  
**Output:** true/false  

Steps:  
1. If `top_ssd == NULL` → return `true`.  
2. Else → return `false`.

---

### 2. `record_ssd(price)` (Push)

**Input:** `price_ssd`  
**Output:** Updated stack  

Steps:  
1. Create a new node.  
2. Set `newNode_ssd->price_ssd = price_ssd`.  
3. Set `newNode_ssd->next_ssd = top_ssd`.  
4. Set `top_ssd = newNode_ssd`.  
5. Print “Price recorded successfully.”

---

### 3. `removePrice_ssd()` (Pop)

**Input:** none  
**Output:** Removed price or -1 if stack empty  

Steps:  
1. If `isEmpty_ssd()` is true:  
   - Print “No prices to remove.”  
   - Return `-1`.  
2. Else:  
   - Store `top_ssd` in `temp_ssd`.  
   - Store `temp_ssd->price_ssd` in `removed_ssd`.  
   - Move `top_ssd = top_ssd->next_ssd`.  
   - `delete temp_ssd`.  
   - Return `removed_ssd`.

---

### 4. `latest_ssd()` (Peek)

**Input:** none  
**Output:** Top price or -1 if empty  

Steps:  
1. If `isEmpty_ssd()` is true:  
   - Print “No prices recorded.”  
   - Return `-1`.  
2. Else:  
   - Return `top_ssd->price_ssd`.

---

### 5. `main()` (Menu)

1. Loop forever:
   - Display menu:
     - 1. Record new price
     - 2. Remove latest price
     - 3. Show latest price
     - 4. Check if empty
     - 5. Exit
   - Take user choice.
   - Perform corresponding stack operation.
2. Exit when user enters `5`.

---

## 💻 C++ Program

```cpp
#include <iostream>
using namespace std;

// Node structure
class Node_ssd 
{
public:
    int price_ssd;
    Node_ssd* next_ssd;
};

Node_ssd* top_ssd = NULL;

// Check if stack is empty
bool isEmpty_ssd() {
    return top_ssd == NULL;
}

// record(price) → Push operation
void record_ssd(int price_ssd) {
    Node_ssd* newNode_ssd = new Node_ssd();
    newNode_ssd->price_ssd = price_ssd;
    newNode_ssd->next_ssd = top_ssd;
    top_ssd = newNode_ssd;
    cout << "Price " << price_ssd << " recorded successfully.\n";
}

// remove() → Pop operation
int removePrice_ssd() {
    if (isEmpty_ssd()) {
        cout << "No prices to remove.\n";
        return -1;
    }

    Node_ssd* temp_ssd = top_ssd;
    int removed_ssd = temp_ssd->price_ssd;
    top_ssd = top_ssd->next_ssd;
    delete temp_ssd;

    return removed_ssd;
}

// latest() → Peek operation
int latest_ssd() {
    if (isEmpty_ssd()) {
        cout << "No prices recorded.\n";
        return -1;
    }
    return top_ssd->price_ssd;
}

int main() {
    int choice_ssd, value_ssd;

    while (true) {
        cout << "\n--- Stock Price Tracker ---\n";
        cout << "1. Record new price\n";
        cout << "2. Remove latest price\n";
        cout << "3. Show latest price\n";
        cout << "4. Check if empty\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1:
                cout << "Enter price: ";
                cin >> value_ssd;
                record_ssd(value_ssd);
                break;

            case 2:
                value_ssd = removePrice_ssd();
                if (value_ssd != -1)
                    cout << "Removed price: " << value_ssd << endl;
                break;

            case 3:
                value_ssd = latest_ssd();
                if (value_ssd != -1)
                    cout << "Latest price: " << value_ssd << endl;
                break;

            case 4:
                if (isEmpty_ssd())
                    cout << "No prices recorded. Stack is empty.\n";
                else
                    cout << "Prices are recorded. Stack is NOT empty.\n";
                break;

            case 5:
                cout << "Exiting...\n";
                return 0;

            default:
                cout << "Invalid choice! Try again.\n";
        }
    }

    return 0;
}
```

---

## 🧪 Example Execution

```text
--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 1
Enter price: 100
Price 100 recorded successfully.

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 1
Enter price: 200
Price 200 recorded successfully.

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 1
Enter price: 300
Price 300 recorded successfully.

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 1
Enter price: 400
Price 400 recorded successfully.

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 1
Enter price: 500
Price 500 recorded successfully.

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 3
Latest price: 500

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 2
Removed price: 500

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 3
Latest price: 400

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 4
Prices are recorded. Stack is NOT empty.

--- Stock Price Tracker ---
1. Record new price
2. Remove latest price
3. Show latest price
4. Check if empty
5. Exit
Enter your choice: 5
Exiting...
```

---

## 🧠 Memory / Stack Visualization

After recording prices: `100, 200, 300, 400, 500`

The **stack (linked list)** looks like:

```text
top_ssd →
[500] → [400] → [300] → [200] → [100] → NULL
```

- Latest price = 500 (top of stack)

After one `removePrice_ssd()`:

```text
Removed: 500

top_ssd →
[400] → [300] → [200] → [100] → NULL
```

- Now latest price = 400  
- Remaining prices still stored below.

---

## ✅ Conclusion

In this assignment, a **stack** was implemented using a **singly linked list** to track stock prices:

- `record_ssd()` adds a new price at the **top** (push).  
- `removePrice_ssd()` removes the **most recent** price (pop).  
- `latest_ssd()` returns the **current latest** price without modifying the stack (peek).  
- `isEmpty_ssd()` checks if there are any recorded prices.

This demonstrates:

- Practical use of **stack operations** in a real-world scenario (stock price history).  
- Use of **dynamic memory allocation** and **linked lists** to implement stack behavior efficiently without fixed size.
