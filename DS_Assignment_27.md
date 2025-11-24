# 🧩 Assignment 27: **Pizza Parlour Simulation Using Circular Queue**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  
This assignment simulates a Pizza Parlour that accepts a maximum of n orders. Orders are served strictly on **FCFS (First-Come, First-Served)** basis using a **Circular Queue**, and once placed, an order cannot be cancelled.

## 📝 Problem Statement
Write a C++ program to maintain pizza orders using a **circular queue**.  
The system should:  
- Allow customer to **place orders (enqueue)**  
- Serve orders strictly in **arrival order (dequeue)**  
- Prevent cancellations  
- Handle **queue full/empty** situations  
- Display current pending orders  
- Exit when user chooses  

## 📘 Theory
A **circular queue** is an improved version of a linear queue.  
In a normal queue, space is wasted when `front` moves forward after deletions.  
To overcome this, circular queue makes the queue **wrap around** using modulo `%`, allowing continuous use of array space.

### 🔹 Why Circular Queue for Pizza Orders?
- Fixed-size order capacity  
- FCFS behaviour  
- Efficient use of memory  
- Perfect for real-time counters, call centers, restaurants  

### 🔹 Important Conditions
- **Queue Empty:** `front == -1`  
- **Queue Full:** `front == (rear + 1) % MAX`  
- **Enqueue:** Insert at `(rear + 1) % MAX`  
- **Dequeue:** Remove from `front` and update `front = (front + 1) % MAX`  

### 🔹 Real-Life Relevance
- Pizza shops, ticket booking, customer service queues  
- Any system where tasks are done **in order of arrival**  

## 📊 Algorithm
1. Initialize circular queue with `front = rear = -1`  
2. **Place Order**  
   - If full → show "Queue Full"  
   - Else insert at next circular position  
3. **Serve Order**  
   - If empty → show "No orders"  
   - Else remove order at front  
4. **Display Orders**  
   - Traverse circularly from `front` to `rear`  
5. Repeat until user selects Exit  

## 💻 CODE

```cpp
    #include <iostream>
    using namespace std;
    #define MAX_SSD 5
    class PizzaParlour_ssd {
        int orders_ssd[MAX_SSD];
        int front_ssd, rear_ssd;
    public:
        PizzaParlour_ssd() { front_ssd = -1; rear_ssd = -1; }
        bool isFull_ssd() { return (front_ssd == (rear_ssd + 1) % MAX_SSD); }
        bool isEmpty_ssd() { return (front_ssd == -1); }
        void placeOrder_ssd(int orderNo_ssd) {
            if (isFull_ssd()) { cout << "Cannot place order. Queue is FULL!\n"; return; }
            if (isEmpty_ssd()) front_ssd = rear_ssd = 0;
            else rear_ssd = (rear_ssd + 1) % MAX_SSD;
            orders_ssd[rear_ssd] = orderNo_ssd;
            cout << "Order " << orderNo_ssd << " placed successfully.\n";
        }
        void serveOrder_ssd() {
            if (isEmpty_ssd()) { cout << "No orders to serve!\n"; return; }
            cout << "Order " << orders_ssd[front_ssd] << " served.\n";
            if (front_ssd == rear_ssd) front_ssd = rear_ssd = -1;
            else front_ssd = (front_ssd + 1) % MAX_SSD;
        }
        void display_ssd() {
            if (isEmpty_ssd()) { cout << "No orders in the queue.\n"; return; }
            cout << "Current Orders: ";
            int i = front_ssd;
            while (true) {
                cout << orders_ssd[i] << " ";
                if (i == rear_ssd) break;
                i = (i + 1) % MAX_SSD;
            }
            cout << "\n";
        }
    };
    int main() {
        PizzaParlour_ssd pp_ssd;
        int choice_ssd, orderNo_ssd;
        while (true) {
            cout << "\n--- Pizza Parlour ---\n";
            cout << "1. Place Order\n2. Serve Order\n3. Display Orders\n4. Exit\nEnter your choice: ";
            cin >> choice_ssd;
            switch (choice_ssd) {
                case 1: cout << "Enter Order Number: "; cin >> orderNo_ssd; pp_ssd.placeOrder_ssd(orderNo_ssd); break;
                case 2: pp_ssd.serveOrder_ssd(); break;
                case 3: pp_ssd.display_ssd(); break;
                case 4: cout << "Exiting...\n"; return 0;
                default: cout << "Invalid choice. Try again.\n";
            }
        }
    }
```


## 🧮 Example Execution
--- Pizza Parlour ---  
1. Place Order  
2. Serve Order  
3. Display Orders  
4. Exit  
Enter your choice: 1  
Enter Order Number: 101  
Order 101 placed successfully.  

--- Pizza Parlour ---  
Enter your choice: 1  
Enter Order Number: 102  
Order 102 placed successfully.  

--- Pizza Parlour ---  
Enter your choice: 1  
Enter Order Number: 103  
Order 103 placed successfully.  

--- Pizza Parlour ---  
Enter your choice: 2  
Order 101 served.  

--- Pizza Parlour ---  
Enter your choice: 2  
Order 102 served.  

--- Pizza Parlour ---  
Enter your choice: 3  
Current Orders: 103  

--- Pizza Parlour ---  
Enter your choice: 2  
Order 103 served.  

--- Pizza Parlour ---  
Enter your choice: 3  
No orders in the queue.  

--- Pizza Parlour ---  
Enter your choice: 4  
Exiting...

## 🧠 Circular Queue Visualization
Initial: front = -1, rear = -1  
After 3 orders: [101, 102, 103]  
After serving 2: front moves to 103  
After serving last: queue empty  

## ✅ Conclusion
This assignment demonstrates the implementation of a **circular queue** to manage pizza orders efficiently.  
Key learnings include:  
- Circular queue logic using modulo  
- FCFS serving mechanism  
- Avoiding wasted space in linear queues  
- Real-life simulation of fast-food order management  
This strengthens queue concepts and practical application of array-based circular data structures.
