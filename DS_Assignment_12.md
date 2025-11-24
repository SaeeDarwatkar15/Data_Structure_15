# 🧩 Assignment 12: Ticket Reservation System for Galaxy Multiplex Using Doubly Circular Linked List

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment implements a **ticket reservation system** for **Galaxy Multiplex** using an **array of doubly circular linked lists**, one per row, to manage **seat availability, booking, and cancellation**.

---

## 🎯 Problem Statement

The **Galaxy Multiplex** has:

- **8 rows**
- **8 seats in each row**

A **doubly circular linked list** is used to track **seat availability** in each row.  
An **array of head pointers** (`head_ssd[8]`) stores the starting node for each row.

Initially, some seats are **randomly booked** using a simple formula.

The system must support:

1. **Display** the current list of available and booked seats.  
2. **Book** one or more seats as per customer request.  
3. **Cancel** an existing booking for a selected seat.

---

## 📘 Theory

### 🔹 Doubly Circular Linked List

A **doubly circular linked list** is a linked list where:

- Each node has:
  - `data` (here: seat number and booking status)
  - `next` pointer → next node
  - `prev` pointer → previous node
- The **last node's `next`** points back to the **head**.
- The **head's `prev`** points to the **last node**.

This structure allows:

- Easy traversal **forward and backward** in a row.
- Circular behavior → no need to check for `NULL` while looping (use `do...while`).

---

### 🔹 Node Structure (Seat)

Each **seat** is represented by the following structure:

```cpp
struct Seat_ssd {
    int seat_no_ssd;   // Seat number (1–8)
    int status_ssd;    // 0 = available, 1 = booked
    Seat_ssd* next_ssd;
    Seat_ssd* prev_ssd;
};
```

### 🔹 Data Representation

- `head_ssd[8]` → Array of pointers, one for each row (Row 1 to Row 8).
- Each `head_ssd[i]` points to a **doubly circular linked list** of 8 seats.

---

### 🔹 Initial Random Booking

Seats are **pre-booked randomly** using:

```cpp
node_ssd->status_ssd = ((r_ssd + s_ssd) % 3 == 0) ? 1 : 0;
```

This creates a pattern of **available (A)** and **booked (B)** seats across the multiplex.

---

### 🔹 Operations

1. **Create Seats**
   - For each row, create 8 seat nodes.
   - Link them as a **doubly circular linked list**.

2. **Display Seat Layout**
   - For each row:
     - Traverse circularly from `head_ssd[row]` back to head.
     - Print `[A#]` for available and `[B#]` for booked.

3. **Find Seat**
   - Given `row` and `seat_no`, traverse that row’s list.
   - Return pointer to the matching node.

4. **Book Seats**
   - Accept row and number of seats.
   - For each seat number:
     - Check if **exists** and **not already booked**.
     - Mark `status_ssd = 1` if available.

5. **Cancel Booking**
   - Accept row and seat number.
   - Mark `status_ssd = 0` if currently booked.

---

## 🧮 Algorithm (High-Level)

### 1️⃣ Create Seats

For each row `r` from 0 to 7:
1. Initialize `head_ssd[r] = NULL`.
2. For each seat `s` from 1 to 8:
   - Create new `Seat_ssd` node.
   - Assign seat number and random booking status.
   - Link nodes to form a **doubly circular linked list**.

---

### 2️⃣ Display Seats

For each row:
1. Set `temp` to `head_ssd[row]`.
2. Use `do...while` loop to traverse from head till you come back to head.
3. Print:
   - `[A#]` → if `status_ssd == 0`
   - `[B#]` → if `status_ssd == 1`

---

### 3️⃣ Book Seats

1. Input `row`, `num_ssd` (number of seats to book).
2. For each seat request:
   - Input seat number.
   - Call `findSeat_ssd(row, seat_no)`:
     - If not found → show message.
     - If already booked → show message.
     - Else → set `status_ssd = 1` (booked).

---

### 4️⃣ Cancel Booking

1. Input `row`, `seat_no`.
2. Find seat using `findSeat_ssd`.
3. If found and status is `1` (booked), set to `0` (available).

---

## 💻 C++ Program

```cpp
#include <iostream>
using namespace std;

struct Seat_ssd {
    int seat_no_ssd;
    int status_ssd; // 0 = available, 1 = booked
    Seat_ssd* next_ssd;
    Seat_ssd* prev_ssd;
};

Seat_ssd* head_ssd[8]; // 8 rows of seats

// Function to create seats
void createSeats_ssd() {
    for (int r_ssd = 0; r_ssd < 8; ++r_ssd) {
        head_ssd[r_ssd] = NULL;
        Seat_ssd* last_ssd = NULL;
        for (int s_ssd = 1; s_ssd <= 8; ++s_ssd) {
            Seat_ssd* node_ssd = new Seat_ssd;
            node_ssd->seat_no_ssd = s_ssd;

            // Random booking pattern: some seats booked initially
            node_ssd->status_ssd = ((r_ssd + s_ssd) % 3 == 0) ? 1 : 0;

            node_ssd->next_ssd = node_ssd->prev_ssd = NULL;

            if (head_ssd[r_ssd] == NULL) {
                head_ssd[r_ssd] = node_ssd;
                node_ssd->next_ssd = node_ssd->prev_ssd = node_ssd;
                last_ssd = node_ssd;
            } else {
                node_ssd->prev_ssd = last_ssd;
                node_ssd->next_ssd = head_ssd[r_ssd];
                last_ssd->next_ssd = node_ssd;
                head_ssd[r_ssd]->prev_ssd = node_ssd;
                last_ssd = node_ssd;
            }
        }
    }
}

// Display seat layout
void displaySeats_ssd() {
    cout << "\n------ Galaxy Multiplex Seat Layout ------\n";
    for (int i_ssd = 0; i_ssd < 8; i_ssd++) {
        cout << "Row " << i_ssd + 1 << ": ";
        Seat_ssd* temp_ssd = head_ssd[i_ssd];
        if (!temp_ssd) continue;
        do {
            cout << "[" << (temp_ssd->status_ssd ? 'B' : 'A')
                 << temp_ssd->seat_no_ssd << "] ";
            temp_ssd = temp_ssd->next_ssd;
        } while (temp_ssd != head_ssd[i_ssd]);
        cout << endl;
    }
    cout << "Legend: [A] = Available, [B] = Booked\n";
}

// Find a specific seat
Seat_ssd* findSeat_ssd(int row_ssd, int seat_no_ssd) {
    if (row_ssd < 1 || row_ssd > 8 || seat_no_ssd < 1 || seat_no_ssd > 8)
        return NULL;

    Seat_ssd* cur_ssd = head_ssd[row_ssd - 1];
    if (!cur_ssd) return NULL;

    do {
        if (cur_ssd->seat_no_ssd == seat_no_ssd)
            return cur_ssd;
        cur_ssd = cur_ssd->next_ssd;
    } while (cur_ssd != head_ssd[row_ssd - 1]);

    return NULL;
}

// Book seats
void bookSeats_ssd() {
    int row_ssd, num_ssd;
    cout << "\nEnter Row number (1-8): ";
    cin >> row_ssd;
    if (row_ssd < 1 || row_ssd > 8) {
        cout << "Invalid row.\n";
        return;
    }

    cout << "Enter number of seats to book: ";
    cin >> num_ssd;

    for (int i_ssd = 0; i_ssd < num_ssd; i_ssd++) {
        int s_ssd;
        cout << "Enter Seat number (1-8): ";
        cin >> s_ssd;

        Seat_ssd* seatNode_ssd = findSeat_ssd(row_ssd, s_ssd);
        if (!seatNode_ssd)
            cout << "Seat not found.\n";
        else if (seatNode_ssd->status_ssd == 1)
            cout << "Seat already booked.\n";
        else {
            seatNode_ssd->status_ssd = 1;
            cout << "Seat " << s_ssd << " booked successfully.\n";
        }
    }
}

// Cancel booking
void cancelBooking_ssd() {
    int row_ssd, s_ssd;
    cout << "\nEnter Row number (1-8): ";
    cin >> row_ssd;
    cout << "Enter Seat number (1-8): ";
    cin >> s_ssd;

    Seat_ssd* seatNode_ssd = findSeat_ssd(row_ssd, s_ssd);
    if (!seatNode_ssd)
        cout << "Seat not found.\n";
    else if (seatNode_ssd->status_ssd == 0)
        cout << "Seat not booked.\n";
    else {
        seatNode_ssd->status_ssd = 0;
        cout << "Booking cancelled for Seat " << s_ssd << ".\n";
    }
}

// Main menu
int main() {
    createSeats_ssd();
    int choice_ssd;

    while (true) {
        cout << "\n========== Galaxy Multiplex Ticket System ==========\n";
        cout << "1. Display Available Seats\n";
        cout << "2. Book Seats\n";
        cout << "3. Cancel Booking\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice_ssd;

        switch (choice_ssd) {
            case 1:
                displaySeats_ssd();
                break;
            case 2:
                bookSeats_ssd();
                break;
            case 3:
                cancelBooking_ssd();
                break;
            case 4:
                cout << "Thank you for visiting Galaxy Multiplex!\n";
                return 0;
            default:
                cout << "Invalid choice. Try again.\n";
        }
    }
}
```

---

## 🧪 Example Execution (Sample)

```text
========== Galaxy Multiplex Ticket System ==========
1. Display Available Seats
2. Book Seats
3. Cancel Booking
4. Exit
Enter your choice: 1

------ Galaxy Multiplex Seat Layout ------
Row 1: [A1] [A2] [B3] [A4] [A5] [B6] [A7] [A8]
Row 2: [A1] [B2] [A3] [A4] [B5] [A6] [A7] [B8]
Row 3: [B1] [A2] [A3] [B4] [A5] [A6] [B7] [A8]
Row 4: [A1] [A2] [B3] [A4] [A5] [B6] [A7] [A8]
Row 5: [A1] [B2] [A3] [A4] [B5] [A6] [A7] [B8]
Row 6: [B1] [A2] [A3] [B4] [A5] [A6] [B7] [A8]
Row 7: [A1] [A2] [B3] [A4] [A5] [B6] [A7] [A8]
Row 8: [A1] [B2] [A3] [A4] [B5] [A6] [A7] [B8]
Legend: [A] = Available, [B] = Booked

========== Galaxy Multiplex Ticket System ==========
1. Display Available Seats
2. Book Seats
3. Cancel Booking
4. Exit
Enter your choice: 2

Enter Row number (1-8): 1
Enter number of seats to book: 1
Enter Seat number (1-8): 1
Seat 1 booked successfully.

========== Galaxy Multiplex Ticket System ==========
Enter your choice: 1

------ Galaxy Multiplex Seat Layout ------
Row 1: [B1] [A2] [B3] [A4] [A5] [B6] [A7] [A8]
... (other rows same as before)

========== Galaxy Multiplex Ticket System ==========
Enter your choice: 3

Enter Row number (1-8): 1
Enter Seat number (1-8): 1
Booking cancelled for Seat 1.

========== Galaxy Multiplex Ticket System ==========
Enter your choice: 1

------ Galaxy Multiplex Seat Layout ------
Row 1: [A1] [A2] [B3] [A4] [A5] [B6] [A7] [A8]
... (other rows unchanged)

========== Galaxy Multiplex Ticket System ==========
Enter your choice: 4
Thank you for visiting Galaxy Multiplex!
```

---

## 🧠 Memory Visualization

### Data Structure Overview

- `head_ssd[8]` → array of 8 pointers (one per row).
- Each `head_ssd[i]` → points to **first seat node** of that row.
- Nodes are linked in a **doubly circular** fashion:

```text
Row 1 (example):

[A1]<=>[A2]<=>[B3]<=>[A4]<=>[A5]<=>[B6]<=>[A7]<=>[A8]
 ^                                         |
 |_________________________________________|
(Circular, with both next and prev links)
```

Each node:

| Field        | Meaning                    |
|-------------|----------------------------|
| seat_no_ssd | Seat number (1 to 8)       |
| status_ssd  | 0 = Available, 1 = Booked  |
| next_ssd    | Pointer to next seat       |
| prev_ssd    | Pointer to previous seat   |

---

## ✅ Conclusion

This assignment demonstrates:

- Use of **doubly circular linked lists** to model a real-world system (multiplex seat booking).  
- How to:
  - Create structured seat layouts.
  - Display availability in a user-friendly format.
  - **Book** and **cancel** seats safely (with checks).
- Use of an **array of head pointers** to handle multiple rows efficiently.

This provides a practical example of applying **linked list concepts** to real-life applications such as **ticket reservation systems**.

