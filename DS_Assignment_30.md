# 🧩 Assignment 30: **Call Center Queue System Using Linked List**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  
This assignment implements a **Call Center System** where customer calls are handled on a **First-Come, First-Served (FCFS)** basis using a **Linked List Queue**. New calls are enqueued as they arrive, and call center agents dequeue calls to assist customers. If the queue is empty, the system prints a waiting message.

## 📝 Problem Statement
Design a program that simulates a **call center queue** using a **linked list**. The system should:  
- Receive new customer calls (**enqueue**)  
- Assist next customer call (**dequeue**)  
- Display all current calls waiting in queue  
- Exit when required  
This system must strictly follow **FCFS order**.

## 📘 Theory
A **linked list queue** is an ideal structure for real-time call handling because it allows dynamic addition and removal of nodes without fixed size limitations.  
### Characteristics of a Linked List Queue  
- **front** → first node; call to be served next  
- **rear** → last node; newest call  
- **enqueue** → add new node at rear in constant time  
- **dequeue** → remove node from front in constant time  
- Queue is empty when **front = nullptr**  
### Advantages  
- No overflow (unless memory full)  
- Perfect for real-time customer support systems  
- Flexible, dynamic, sequential, and efficient  

## 💻 CODE

```cpp
    #include <iostream>
    #include <string>
    using namespace std;
    struct Node_ssd {
        string callName_ssd;
        Node_ssd* next_ssd;
    };
    class CallQueue_ssd {
    private:
        Node_ssd* front_ssd;
        Node_ssd* rear_ssd;
    public:
        CallQueue_ssd() { front_ssd = rear_ssd = nullptr; }
        void enqueueCall_ssd(const string &caller_ssd) {
            Node_ssd* newNode_ssd = new Node_ssd();
            newNode_ssd->callName_ssd = caller_ssd;
            newNode_ssd->next_ssd = nullptr;
            if (rear_ssd == nullptr) front_ssd = rear_ssd = newNode_ssd;
            else { rear_ssd->next_ssd = newNode_ssd; rear_ssd = newNode_ssd; }
            cout << "Call from '" << caller_ssd << "' added to the queue.\n";
        }
        void assistCall_ssd() {
            if (front_ssd == nullptr) { cout << "No calls in queue. Waiting...\n"; return; }
            Node_ssd* temp_ssd = front_ssd;
            cout << "Assisting call from '" << temp_ssd->callName_ssd << "'.\n";
            front_ssd = front_ssd->next_ssd;
            if (front_ssd == nullptr) rear_ssd = nullptr;
            delete temp_ssd;
        }
        void displayQueue_ssd() {
            if (front_ssd == nullptr) { cout << "Queue is empty.\n"; return; }
            cout << "Current Call Queue: ";
            Node_ssd* temp_ssd = front_ssd;
            while (temp_ssd != nullptr) {
                cout << "'" << temp_ssd->callName_ssd << "' ";
                temp_ssd = temp_ssd->next_ssd;
            }
            cout << "\n";
        }
    };
    int main() {
        CallQueue_ssd callCenter_ssd;
        int choice_ssd;
        string caller_ssd;
        while (true) {
            cout << "\n--- Call Center System ---\n";
            cout << "1. Receive New Call (Enqueue)\n";
            cout << "2. Assist Next Call (Dequeue)\n";
            cout << "3. Display Call Queue\n";
            cout << "4. Exit\n";
            cout << "Enter choice: ";
            cin >> choice_ssd;
            switch (choice_ssd) {
                case 1:
                    cout << "Enter Caller Name: ";
                    cin >> caller_ssd;
                    callCenter_ssd.enqueueCall_ssd(caller_ssd);
                    break;
                case 2:
                    callCenter_ssd.assistCall_ssd();
                    break;
                case 3:
                    callCenter_ssd.displayQueue_ssd();
                    break;
                case 4:
                    cout << "Exiting Call Center System...\n";
                    return 0;
                default:
                    cout << "Invalid choice! Try again.\n";
            }
        }
    }
```


## 🧮 Example Execution
--- Call Center System ---  
Enter choice: 1 → "Jay" added  
Enter choice: 1 → "Ajay" added  
Enter choice: 1 → "Vijay" added  
Enter choice: 1 → "Ram" added  
Enter choice: 2 → Assisting "Jay"  
Enter choice: 3 → 'Ajay' 'Vijay' 'Ram'  
Enter choice: 2 → Assisting "Ajay"  
Enter choice: 3 → 'Vijay' 'Ram'  
Enter choice: 4 → Exiting Call Center System...


## 🧠 Memory Visualization (Linked List Queue)
After four enqueues:  
front → [Jay] → [Ajay] → [Vijay] → [Ram] → null ← rear  
After serving Jay:  
front → [Ajay] → [Vijay] → [Ram] → null ← rear  
After serving Ajay:  
front → [Vijay] → [Ram] → null ← rear  
If queue becomes empty:  
front = rear = null  
This shows dynamic node linking, continuous insertion at rear, and deletion at front following strict FCFS rules.


## ✅ Conclusion
This assignment successfully demonstrates how a **Linked List Queue** can manage real-time incoming calls in a call center using efficient enqueue and dequeue operations. The linked list structure removes fixed-size limitations of arrays and ensures smooth call handling even with unpredictable traffic. The FCFS model guarantees fairness as every caller is served in exact arrival order. This exercise strengthens understanding of:  
- Dynamic node allocation and deletion  
- Pointer manipulation  
- Real-time queue simulation  
- Practical application of linked lists in customer service systems  
- Memory flow and visualization of dynamic queues  
Overall, it reinforces how data structures directly support real-world operations like call management, customer flow regulation, and service queue automation.
