# 🧱 Assignment 23: Multiple Stacks Using a Single Array

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

---

## 🎯 Aim

Write a C++ program to **implement multiple stacks (more than two)** using a **single array**, and perform the following operations:

1. **Push**  
2. **Pop**  
3. **Stack Overflow** (when no free space is left in array)  
4. **Stack Underflow** (when trying to pop from an empty stack)  
5. **Display** contents of individual stacks and all stacks  

The program should allow the user to choose **which stack** (by stack number) they want to perform operations on.

---

## 📘 Problem Statement

Implement **k stacks** in a **single array** of fixed size `MAX_SIZE_SSD`.  
Use supporting arrays and indices to manage:

- The **top index** of each stack (`top_ssd[]`)
- A **free list** of available positions in the main array
- A **next index** array (`next_idx_ssd[]`) that works as:
  - Linked list for each stack  
  - Linked list for free slots

All stacks share the **same array** memory instead of separate arrays.

---

## 🧠 Theory

### 🔹 Why Multiple Stacks in One Array?

Instead of allocating **separate arrays** for each stack (which can waste memory), we can store **all stacks in one array** and dynamically share space:

- If one stack is small and another is large, they can **reuse the same pool of free cells**.
- This avoids fixed partitioning like:  
  - Stack 1 → index 0–4  
  - Stack 2 → index 5–9  
  which may cause overflow in one stack even if total array still has free space.

---

### 🔹 Data Structures Used

1. **`arr_ssd[MAX_SIZE_SSD]`**  
   Stores **actual stack values**.

2. **`top_ssd[NUM_STACKS_SSD]`**  
   Stores **top index** for each stack.  
   - `top_ssd[i] = -1` → Stack `i+1` is empty.

3. **`next_idx_ssd[MAX_SIZE_SSD]`**  
   - For **free slots**: works like a **linked free list**.  
   - For **used slots**: connects elements **inside each stack**.

4. **`freeTop_ssd`**  
   - Index of the **first free slot** in `arr_ssd`.  
   - If `freeTop_ssd == -1` → **No free space** → **Overflow**.

---

### 🔹 Working of `push` Operation

1. Check if `freeTop_ssd == -1`  
   → No free space → **Stack Overflow**.

2. Otherwise:
   - Take `insert_at = freeTop_ssd`.
   - Move freeTop to next free: `freeTop_ssd = next_idx_ssd[insert_at]`.
   - Store value in `arr_ssd[insert_at]`.
   - Link it to current stack top:  
     `next_idx_ssd[insert_at] = top_ssd[stackNumber - 1]`.
   - Update stack top:  
     `top_ssd[stackNumber - 1] = insert_at`.

---

### 🔹 Working of `pop` Operation

1. Check if `top_ssd[stackNumber - 1] == -1`  
   → **Stack Underflow** (empty stack).

2. Otherwise:
   - Get `top_index = top_ssd[stackNumber - 1]`.
   - Read value `arr_ssd[top_index]`.
   - Move stack top to next element:  
     `top_ssd[stackNumber - 1] = next_idx_ssd[top_index]`.
   - Add freed `top_index` back to **free list**:  
     `next_idx_ssd[top_index] = freeTop_ssd;`  
     `freeTop_ssd = top_index;`.

---

## 🔢 Algorithm (High-Level)

### Initialization

1. Set all `top_ssd[i] = -1` for all stacks.
2. Link all indices in `next_idx_ssd` as a free list:
   - `next_idx_ssd[0] = 1, next_idx_ssd[1] = 2, ..., next_idx_ssd[MAX_SIZE-2] = MAX_SIZE-1`
   - `next_idx_ssd[MAX_SIZE-1] = -1`
3. Set `freeTop_ssd = 0` (first free slot).

---

### Push (stackNumber, value)

1. If `freeTop_ssd == -1` → print **Overflow**.
2. Else:
   - `insert_at = freeTop_ssd`
   - `freeTop_ssd = next_idx_ssd[insert_at]`
   - `arr_ssd[insert_at] = value`
   - `next_idx_ssd[insert_at] = top_ssd[stackNumber - 1]`
   - `top_ssd[stackNumber - 1] = insert_at`

---

### Pop (stackNumber)

1. If `top_ssd[stackNumber - 1] == -1` → print **Underflow**.
2. Else:
   - `top_index = top_ssd[stackNumber - 1]`
   - `popped = arr_ssd[top_index]`
   - `top_ssd[stackNumber - 1] = next_idx_ssd[top_index]`
   - `next_idx_ssd[top_index] = freeTop_ssd`
   - `freeTop_ssd = top_index`
   - Return `popped`.

---

### Display Stack (Top → Bottom)

1. Start from `idx = top_ssd[stackNumber - 1]`
2. While `idx != -1`:
   - Print `arr_ssd[idx]`
   - Move: `idx = next_idx_ssd[idx]`.

---

## 💻 C++ Program – Multiple Stacks Using Single Array

```cpp
#include <iostream>
#include <cstdlib>
#include <cstring>

#define NUM_STACKS_SSD 4    // change as needed
#define MAX_SIZE_SSD   20   // total size of array that stores all stacks

// shared array and metadata
int arr_ssd[MAX_SIZE_SSD];         // actual values
int top_ssd[NUM_STACKS_SSD];       // indexes of tops of stacks
int next_idx_ssd[MAX_SIZE_SSD];    // next index: free-list & stack links
int freeTop_ssd;                   // beginning index of free list

// initialize data structures
void initStacks_ssd() {
    for (int i = 0; i < NUM_STACKS_SSD; ++i) top_ssd[i] = -1;
    for (int i = 0; i < MAX_SIZE_SSD - 1; ++i) next_idx_ssd[i] = i + 1;
    next_idx_ssd[MAX_SIZE_SSD - 1] = -1;
    freeTop_ssd = 0;
}

// check if array is full (no free slot)
bool isFull_ssd() {
    return freeTop_ssd == -1;
}

// check if specific stack is empty (1-based stack number)
bool isEmpty_ssd(int sn_ssd) {
    if (sn_ssd < 1 || sn_ssd > NUM_STACKS_SSD) return true; // invalid treated as empty
    return top_ssd[sn_ssd - 1] == -1;
}

// push value onto stack number sn_ssd (1-based).
// returns 0 on success, -1 on overflow, -2 on invalid stack number
int push_ssd(int sn_ssd, int value_ssd) {
    if (sn_ssd < 1 || sn_ssd > NUM_STACKS_SSD) return -2; // invalid stack number
    if (isFull_ssd()) return -1; // overflow

    // take free index
    int insert_at_ssd = freeTop_ssd;
    freeTop_ssd = next_idx_ssd[insert_at_ssd];

    // store value
    arr_ssd[insert_at_ssd] = value_ssd;

    // link to previous top
    next_idx_ssd[insert_at_ssd] = top_ssd[sn_ssd - 1];
    top_ssd[sn_ssd - 1] = insert_at_ssd;

    return 0;
}

// pop value from stack number sn_ssd (1-based).
// returns 0 on success and sets popped_ssd, -1 on underflow, -2 on invalid stack number
int pop_ssd(int sn_ssd, int &popped_ssd) {
    if (sn_ssd < 1 || sn_ssd > NUM_STACKS_SSD) return -2; // invalid stack number
    if (isEmpty_ssd(sn_ssd)) return -1; // underflow

    int top_index_ssd = top_ssd[sn_ssd - 1];
    popped_ssd = arr_ssd[top_index_ssd];

    // move top to next element in this stack
    top_ssd[sn_ssd - 1] = next_idx_ssd[top_index_ssd];

    // add freed index back to free list
    next_idx_ssd[top_index_ssd] = freeTop_ssd;
    freeTop_ssd = top_index_ssd;

    return 0;
}

// display elements of a stack (from top to bottom)
void displayStack_ssd(int sn_ssd) {
    if (sn_ssd < 1 || sn_ssd > NUM_STACKS_SSD) {
        std::cout << "Invalid stack number. Choose 1 to " << NUM_STACKS_SSD << "\n";
        return;
    }
    int idx_ssd = top_ssd[sn_ssd - 1];
    if (idx_ssd == -1) {
        std::cout << "Stack " << sn_ssd << " is empty.\n";
        return;
    }
    std::cout << "Stack " << sn_ssd << " (top -> bottom): ";
    while (idx_ssd != -1) {
        std::cout << arr_ssd[idx_ssd] << " ";
        idx_ssd = next_idx_ssd[idx_ssd];
    }
    std::cout << "\n";
}

// display all stacks and free slot count
void displayAll_ssd() {
    for (int i = 1; i <= NUM_STACKS_SSD; ++i) {
        displayStack_ssd(i);
    }
    int countFree_ssd = 0;
    int idx_ssd = freeTop_ssd;
    while (idx_ssd != -1) { countFree_ssd++; idx_ssd = next_idx_ssd[idx_ssd]; }
    std::cout << "Free slots available: " << countFree_ssd << " out of " << MAX_SIZE_SSD << "\n";
}

int main() {
    initStacks_ssd();

    int choice_ssd = 0;
    while (true) {
        std::cout << "\n--- Multiple Stacks using single array ---\n";
        std::cout << "Number of stacks: " << NUM_STACKS_SSD << ", Array size: " << MAX_SIZE_SSD << "\n";
        std::cout << "1. Push\n2. Pop\n3. Display a stack\n4. Display all stacks\n5. Check Overflow (full)\n6. Check Underflow for a stack\n7. Exit\n";
        std::cout << "Enter choice: ";
        if (!(std::cin >> choice_ssd)) {
            std::cin.clear();
            std::cin.ignore(10000, '\n');
            std::cout << "Invalid input. Try again.\n";
            continue;
        }

        if (choice_ssd == 1) {
            int sn_ssd, value_ssd;
            std::cout << "Enter stack number (1-" << NUM_STACKS_SSD << "): ";
            std::cin >> sn_ssd;
            std::cout << "Enter value to push: ";
            std::cin >> value_ssd;
            int status_ssd = push_ssd(sn_ssd, value_ssd);
            if (status_ssd == -1) std::cout << "Stack Overflow: no free space to push " << value_ssd << ".\n";
            else if (status_ssd == -2) std::cout << "Invalid stack number.\n";
            else std::cout << "Pushed " << value_ssd << " to stack " << sn_ssd << ".\n";
        }
        else if (choice_ssd == 2) {
            int sn_ssd;
            std::cout << "Enter stack number (1-" << NUM_STACKS_SSD << "): ";
            std::cin >> sn_ssd;
            int popped_ssd;
            int status_ssd = pop_ssd(sn_ssd, popped_ssd);
            if (status_ssd == -1) std::cout << "Stack Underflow: stack " << sn_ssd << " is empty.\n";
            else if (status_ssd == -2) std::cout << "Invalid stack number.\n";
            else std::cout << "Popped " << popped_ssd << " from stack " << sn_ssd << ".\n";
        }
        else if (choice_ssd == 3) {
            int sn_ssd;
            std::cout << "Enter stack number (1-" << NUM_STACKS_SSD << "): ";
            std::cin >> sn_ssd;
            displayStack_ssd(sn_ssd);
        }
        else if (choice_ssd == 4) {
            displayAll_ssd();
        }
        else if (choice_ssd == 5) {
            if (isFull_ssd()) std::cout << "Array is full — Stack Overflow state (no free space).\n";
            else std::cout << "There is free space available for pushes.\n";
        }
        else if (choice_ssd == 6) {
            int sn_ssd;
            std::cout << "Enter stack number (1-" << NUM_STACKS_SSD << "): ";
            std::cin >> sn_ssd;
            if (isEmpty_ssd(sn_ssd)) std::cout << "Stack " << sn_ssd << " is empty → Underflow.\n";
            else std::cout << "Stack " << sn_ssd << " is NOT empty.\n";
        }
        else if (choice_ssd == 7) {
            std::cout << "Exiting...\n";
            break;
        }
        else {
            std::cout << "Invalid choice. Try again.\n";
        }
    }

    return 0;
}

## 🧪 Example Execution

--- Multiple Stacks using single array ---
Number of stacks: 4, Array size: 20
1. Push
2. Pop
3. Display a stack
4. Display all stacks
5. Check Overflow (full)
6. Check Underflow for a stack
7. Exit
Enter choice: 1
Enter stack number (1-4): 1
Enter value to push: 10
Pushed 10 to stack 1.

... (more pushes) ...

Enter choice: 4
Stack 1 (top -> bottom): 20 10 
Stack 2 (top -> bottom): 40 30 
Stack 3 (top -> bottom): 60 50 
Stack 4 (top -> bottom): 80 70 
Free slots available: 12 out of 20

Enter choice: 7
Exiting...


## 🧩 Memory Visualization 

arr_ssd[] holds all elements from all stacks.
top_ssd[] holds starting indices for each stack.
next_idx_ssd[]:
    For used elements: acts like next pointer inside that stack.
    For free positions: forms a linked list of free indexes starting at freeTop_ssd.

So you’ve successfully:

Implemented k stacks in a single array
Handled push, pop, overflow, underflow, display
Used an efficient linked-list–like approach inside an array 💪