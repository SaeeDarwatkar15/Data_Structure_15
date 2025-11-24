# 🧩 Assignment 29: **Implementation of Multiple Queues Using Single Array**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  
This assignment implements **two queues** using a **single array**, dividing the array into two equal partitions. Each partition behaves like an independent circular queue. Operations performed: **Add Queue**, **Delete from Queue**, and **Display Queue**.

## 📝 Problem Statement
Write a program to implement **two queues using one array**. The user can:  
1. Add an element to Queue 1 or Queue 2  
2. Delete an element from Queue 1 or Queue 2  
3. Display elements of Queue 1 or Queue 2  
The system should use **fixed partitioning** and **circular queue logic** inside each partition.

## 📘 Theory
A **multiple-queue system using one array** saves memory. The array is divided into two equal ranges:  
- Queue 1 → Index `0` to `partition_size - 1`  
- Queue 2 → Index `partition_size` to `MAX_SIZE - 1`  
Both queues maintain **separate front and rear pointers**, use **circular queue logic**, and operate independently.  
### Key Conditions  
- **Queue Empty:** `front == -1`  
- **Queue Full:** next rear position = front  
### Circular Movement  
If rear reaches end of its partition → wrap to start using modulo.  
This design demonstrates how to manage **multiple queues** without creating multiple arrays.

## 💻 CODE

```cpp
    #include <iostream>
    #include <iomanip>
    using namespace std;
    #define MAX_SIZE_SSD 10
    #define NUM_QUEUES_SSD 2
    int arr_ssd[MAX_SIZE_SSD];
    int front_ssd[NUM_QUEUES_SSD];
    int rear_ssd[NUM_QUEUES_SSD];
    int partition_size_ssd;
    void initQueues_ssd() {
        partition_size_ssd = MAX_SIZE_SSD / NUM_QUEUES_SSD;
        for (int i = 0; i < NUM_QUEUES_SSD; ++i) {
            front_ssd[i] = -1;
            rear_ssd[i] = -1;
        }
    }
    int startIndex_ssd(int q_ssd) { return (q_ssd - 1) * partition_size_ssd; }
    int endIndex_ssd(int q_ssd)   { return startIndex_ssd(q_ssd) + partition_size_ssd - 1; }
    bool isEmptyQueue_ssd(int q_ssd) {
        return front_ssd[q_ssd - 1] == -1;
    }
    bool isFullQueue_ssd(int q_ssd) {
        int idx = q_ssd - 1;
        if (front_ssd[idx] == -1) return false;
        int next_rear = (rear_ssd[idx] == endIndex_ssd(q_ssd)) ? startIndex_ssd(q_ssd) : rear_ssd[idx] + 1;
        return next_rear == front_ssd[idx];
    }
    bool enqueue_ssd(int q_ssd, int value_ssd) {
        int idx = q_ssd - 1;
        if (isFullQueue_ssd(q_ssd)) return false;
        if (front_ssd[idx] == -1) front_ssd[idx] = rear_ssd[idx] = startIndex_ssd(q_ssd);
        else rear_ssd[idx] = (rear_ssd[idx] == endIndex_ssd(q_ssd)) ? startIndex_ssd(q_ssd) : rear_ssd[idx] + 1;
        arr_ssd[rear_ssd[idx]] = value_ssd;
        return true;
    }
    pair<bool,int> dequeue_ssd(int q_ssd) {
        int idx = q_ssd - 1;
        if (isEmptyQueue_ssd(q_ssd)) return {false,0};
        int value_ssd = arr_ssd[front_ssd[idx]];
        if (front_ssd[idx] == rear_ssd[idx]) front_ssd[idx] = rear_ssd[idx] = -1;
        else front_ssd[idx] = (front_ssd[idx] == endIndex_ssd(q_ssd)) ? startIndex_ssd(q_ssd) : front_ssd[idx] + 1;
        return {true,value_ssd};
    }
    void displayQueue_ssd(int q_ssd) {
        int idx = q_ssd - 1;
        if (isEmptyQueue_ssd(q_ssd)) {
            cout << "Queue " << q_ssd << " is empty.\n";
            return;
        }
        cout << "Queue " << q_ssd << " (front -> rear): ";
        int i = front_ssd[idx];
        while (true) {
            cout << arr_ssd[i] << " ";
            if (i == rear_ssd[idx]) break;
            i = (i == endIndex_ssd(q_ssd)) ? startIndex_ssd(q_ssd) : i + 1;
        }
        cout << "\n";
    }
    int main() {
        initQueues_ssd();
        int choice_ssd;
        cout << "Two Queues using single array (fixed partitions)\n";
        cout << "Total array size: " << MAX_SIZE_SSD << ", Partition size: " << partition_size_ssd << "\n";
        while (true) {
            cout << "\nMenu:\n1. Add to Queue\n2. Delete from Queue\n3. Display Queue\n4. Exit\nEnter choice: ";
            cin >> choice_ssd;
            if (choice_ssd == 1) {
                int qnum_ssd, val_ssd;
                cout << "Enter queue number (1 or 2): ";
                cin >> qnum_ssd;
                cout << "Enter value to add: ";
                cin >> val_ssd;
                if (enqueue_ssd(qnum_ssd, val_ssd)) cout << "Added " << val_ssd << " to queue " << qnum_ssd << ".\n";
                else cout << "Queue " << qnum_ssd << " Overflow.\n";
            }
            else if (choice_ssd == 2) {
                int qnum_ssd;
                cout << "Enter queue number (1 or 2): ";
                cin >> qnum_ssd;
                auto res = dequeue_ssd(qnum_ssd);
                if (!res.first) cout << "Queue " << qnum_ssd << " Underflow.\n";
                else cout << "Deleted " << res.second << " from queue " << qnum_ssd << ".\n";
            }
            else if (choice_ssd == 3) {
                int qnum_ssd;
                cout << "Enter queue number (1 or 2) to display: ";
                cin >> qnum_ssd;
                displayQueue_ssd(qnum_ssd);
            }
            else if (choice_ssd == 4) {
                cout << "Exiting program.\n";
                break;
            }
            else cout << "Invalid choice.\n";
        }
        return 0;
    }
```


## 🧮 Example Execution
Two Queues using single array (fixed partitions)  
Total array size: 10, Partition size: 5  
Menu:  
1. Add to Queue  
Enter choice: 1  
Enter queue number (1 or 2): 1  
Enter value to add: 10  
Added 10 to queue 1.  
Menu:  
Enter choice: 1  
Enter queue number (1 or 2): 1  
Enter value to add: 20  
Added 20 to queue 1.  
Menu:  
Enter choice: 1  
Enter queue number (1 or 2): 2  
Enter value to add: 30  
Added 30 to queue 2.  
Menu:  
Enter choice: 1  
Enter queue number (1 or 2): 2  
Enter value to add: 40  
Added 40 to queue 2.  
Menu:  
Enter choice: 3  
Enter queue number: 1  
Queue 1 (front -> rear): 10 20  
Menu:  
Enter choice: 3  
Enter queue number: 2  
Queue 2 (front -> rear): 30 40  
Menu:  
Enter choice: 2  
Enter queue number: 1  
Deleted 10 from queue 1.  
Menu:  
Enter choice: 3  
Enter queue number: 1  
Queue 1 (front -> rear): 20  
Menu:  
Enter choice: 4  
Exiting program.

## ✅ Conclusion
This program demonstrates how **two independent queues** can be implemented using **one shared array** by dividing it into fixed partitions and applying **circular queue logic** inside each partition. It strengthens understanding of queue operations, partitioned memory management, overflow/underflow handling, and array-based data structure implementation.
