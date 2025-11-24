# 🧩 Assignment 28: **Ticket Agent Queue Management Using STL Queue**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  

This assignment simulates a **queue of passengers** waiting to see a **ticket agent**. The program allows inserting passengers at the **rear**, viewing/removing the passenger at the **front**, and shows how many passengers are left in the queue when the program ends. The logic is implemented using the **STL `queue` container** in C++.

## 📝 Problem Statement
Write a C++ program that maintains a **queue of passengers** waiting to see a ticket agent. The program user should be able to:
- **Insert** a new passenger at the **rear** of the queue  
- **Display** the passenger at the **front** of the queue  
- **Remove** the passenger at the **front** of the queue  

When the program terminates, it must display:
- The **number of passengers left** in the queue  
- The **list of remaining passengers** (from front to back), if any  

The queue follows **FCFS (First-Come, First-Served)** discipline.

## 📘 Theory

### 🔹 Queue Data Structure
A **queue** is a linear data structure that works on the **FIFO (First In, First Out)** principle:
- The **first element inserted** is the **first element removed**.
- Real-life example: people standing in a line at a ticket counter.

### 🔹 Operations on Queue
Basic operations:
- **Enqueue (push)** → Insert element at the **rear**  
- **Dequeue (pop)** → Remove element from the **front**  
- **Front** → Access the element at the **front** without removing  
- **Empty** → Check if queue has no elements  
- **Size** → Get number of elements  

### 🔹 Using STL `queue<string>`
C++ Standard Template Library provides `queue<T>`:
- Internally uses **deque** or **list**  
- Supports all basic queue operations  
- Very easy for real-life simulations like this assignment  

### 🔹 Application in Ticket Agent System
- Each passenger is represented by a **name (string)**  
- Arrival order = **queue insertion order**  
- Ticket agent always serves the **front** passenger  
- After service, the passenger is **removed** from the queue  

This models real-world service systems like:
- Ticket counters  
- Customer support queues  
- Bank counters  

## 📊 Algorithm

1. Declare `queue<string> q_ssd` for passengers  
2. Repeatedly display a **menu**:
   - 1 → Add Passenger  
   - 2 → Show Front Passenger  
   - 3 → Remove Front Passenger  
   - 4 → Exit  
3. For **Add Passenger**:
   - Read passenger name  
   - If non-empty → `push` into queue  
4. For **Show Front Passenger**:
   - If queue is empty → show message  
   - Else → display `q_ssd.front()`  
5. For **Remove Front Passenger**:
   - If queue is empty → show message  
   - Else → show and `pop` front passenger  
6. For **Exit**:
   - Display number of passengers left → `q_ssd.size()`  
   - If any left → display them from front to back (using temporary queue)  
   - Terminate program  

Time Complexity per operation: **O(1)**  
Space Complexity: **O(n)** (where n = number of passengers)

## 💻 CODE

```cpp
    #include <iostream>
    #include <queue>
    #include <string>
    #include <limits>

    using namespace std;

    int main() {
        queue<string> q_ssd;
        int choice_ssd = 0;

        while (true) {
            cout << "\n--- Ticket Agent Queue ---\n";
            cout << "1. Add Passenger to Queue\n";
            cout << "2. Show Front Passenger\n";
            cout << "3. Remove Front Passenger\n";
            cout << "4. Exit\n";
            cout << "Enter your choice: ";

            if (!(cin >> choice_ssd)) {
                // handle non-integer input
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Invalid input. Please enter a number between 1 and 4.\n";
                continue;
            }

            // consume the newline left by reading choice so getline works later
            cin.ignore(numeric_limits<streamsize>::max(), '\n');

            if (choice_ssd == 1) {
                string name_ssd;
                cout << "Enter passenger name: ";
                getline(cin, name_ssd);
                if (name_ssd.empty()) {
                    cout << "Name cannot be empty. Cancelled.\n";
                } else {
                    q_ssd.push(name_ssd);
                    cout << "Passenger '" << name_ssd << "' added to the queue.\n";
                }
            }
            else if (choice_ssd == 2) {
                if (q_ssd.empty()) {
                    cout << "Queue is empty. No passenger at the front.\n";
                } else {
                    cout << "Passenger at front: " << q_ssd.front() << "\n";
                }
            }
            else if (choice_ssd == 3) {
                if (q_ssd.empty()) {
                    cout << "Queue is empty. No passenger to remove.\n";
                } else {
                    cout << "Passenger '" << q_ssd.front() << "' removed from the queue.\n";
                    q_ssd.pop();
                }
            }
            else if (choice_ssd == 4) {
                cout << "\nProgram ending...\n";
                cout << "Passengers left in queue: " << q_ssd.size() << "\n";
                if (!q_ssd.empty()) {
                    cout << "List of remaining passengers (front -> back):\n";
                    // print without destroying queue
                    queue<string> tmp_ssd = q_ssd;
                    int idx_ssd = 1;
                    while (!tmp_ssd.empty()) {
                        cout << idx_ssd << ". " << tmp_ssd.front() << "\n";
                        tmp_ssd.pop();
                        ++idx_ssd;
                    }
                }
                return 0;
            }
            else {
                cout << "Invalid choice. Try again.\n";
            }
        }

        return 0;
    }
```


## 🧮 Example Execution 

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 1
    Enter passenger name: Jay
    Passenger 'Jay' added to the queue.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 1
    Enter passenger name: Ajay
    Passenger 'Ajay' added to the queue.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 1
    Enter passenger name: Vijay
    Passenger 'Vijay' added to the queue.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 2
    Passenger at front: Jay

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 3
    Passenger 'Jay' removed from the queue.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 2
    Passenger at front: Ajay

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 3
    Passenger 'Ajay' removed from the queue.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 3
    Passenger 'Vijay' removed from the queue.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 2
    Queue is empty. No passenger at the front.

    --- Ticket Agent Queue ---
    1. Add Passenger to Queue
    2. Show Front Passenger
    3. Remove Front Passenger
    4. Exit
    Enter your choice: 4

    Program ending...
    Passengers left in queue: 0

## ✅ Conclusion
This assignment demonstrates how to implement a **simple queue-based ticket system** using the **STL `queue<string>`** in C++. It helps to understand:
- Practical use of a **FIFO queue**  
- Basic operations: **enqueue, dequeue, front, empty, size**  
- Menu-driven user interaction  
- How to safely handle **invalid input** and display the **final state** of the queue  

Such a model closely reflects real-world systems like **ticket counters** and helps strengthen the concepts of **queue data structure** and **STL usage** in C++.
