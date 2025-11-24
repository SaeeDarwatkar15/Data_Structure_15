# 🧩 Assignment 13: Appointment Scheduling Using Singly Linked List

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This assignment implements a **single-day appointment scheduler** using a **singly linked list**, with support for **random initial booking**, **booking**, **cancellation**, and **sorting** by appointment time using both **data swapping** and **pointer manipulation**.

---

## 📝 Problem Statement

Develop a **C++ program** to store and manage an **appointment schedule for a single day**.

- Appointments are stored in a **singly linked list**.
- The schedule is generated using a **defined start time**, **end time**, and **fixed duration** for each slot.
- Initially, each slot is **randomly** set to **Booked** or **Available**.

The system must support:

1. **Display** the list of all time slots with their booking status.  
2. **Book** a new appointment within valid time limits.  
3. **Cancel** an existing appointment after validating correctness.  
4. **Sort** the appointment list **by time using data swapping**.  
5. **Sort** the appointment list **by pointer manipulation** (no data swapping).

---

## 📘 Theory

### 🔹 Linked List for Scheduling

A **singly linked list** is used to represent the appointment schedule, where:

Each node (slot) stores:
- `startHour_ssd` → Start time of the slot (hour)  
- `endHour_ssd` → End time of the slot (hour)  
- `booked_ssd` → Status: `0 = Available`, `1 = Booked`  
- `next_ssd` → Pointer to the next slot  

Advantages:
- Dynamic size
- Easy insertion, traversal, and sorting

---

### 🔹 Time Slot Definition

- Working hours are from **9:00 AM** to **5:00 PM**:
  - `start_ssd = 9`
  - `end_ssd = 17`
  - `duration_ssd = 1` hour
- Slots generated:
  - 9–10, 10–11, 11–12, ..., 16–17

---

### 🔹 Random Booking Initialization

During schedule creation:

```cpp
node_ssd->booked_ssd = rand() % 2;
```

Each slot is randomly assigned as:
- `0` → Available  
- `1` → Booked  

This simulates a partially filled schedule at the start of the day.

---

### 🔹 Supported Operations

1. **Display All Slots**
   - Traverse the list and print each slot time and status.

2. **Book Appointment**
   - Ask user for start time (hour).
   - Search for matching node.
   - If available, mark as **Booked**.

3. **Cancel Appointment**
   - Ask user for start time.
   - If currently booked, mark as **Available**.

4. **Sort by Time (Data Swapping)**
   - Use **Bubble Sort** over the list.
   - Swap **data fields** (time and status) between nodes.

5. **Sort by Time (Pointer Manipulation)**
   - Use **Insertion Sort** on pointers.
   - Build a **new sorted list** by re-linking nodes.
   - No data fields are swapped; only `next_ssd` pointers are updated.

---

## 📊 Algorithm (High-Level)

### A) Create Schedule

1. Input:
   - `start_ssd` (e.g., 9)
   - `end_ssd` (e.g., 17)
   - `duration_ssd` (e.g., 1)
2. For `t` from `start_ssd` to `< end_ssd` step `duration_ssd`:
   - Create new node.
   - Set `startHour_ssd = t`, `endHour_ssd = t + duration_ssd`.
   - Randomly set `booked_ssd = rand() % 2`.
   - Append to linked list.

---

### B) Display Schedule

1. Start at `head_ssd`.
2. For each node:
   - Print `"startHour:00 - endHour:00 => Booked/Available"`.

---

### C) Book Appointment

1. Ask for `start_ssd` (hour).
2. Traverse list:
   - If `node->startHour_ssd == start_ssd`:
     - If not booked → set `booked_ssd = 1`.
     - Else → show "Slot already booked".
3. If not found → "Invalid time entered".

---

### D) Cancel Appointment

1. Ask for `start_ssd`.
2. Traverse list:
   - If slot found:
     - If `booked_ssd == 1` → set to `0` and print success.
     - Else → "Slot not booked yet".
3. If not found → "Invalid time entered".

---

### E) Sort by Data (Bubble Sort)

1. Outer loop: `i` from head to end.  
2. Inner loop: `j = i->next` to end.  
3. If `i->startHour_ssd > j->startHour_ssd` → swap **data fields** using `swapData_ssd()`.

---

### F) Sort by Pointer (Insertion Sort Style)

1. Maintain `sorted_ssd = NULL`.  
2. Take each node from original list:
   - Insert it into `sorted_ssd` at correct position based on `startHour_ssd`.
3. Return `sorted_ssd` as new head.

---

## 💻 C++ Program

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <limits>

using namespace std;

typedef struct Appointment_ssd {
    int startHour_ssd;
    int endHour_ssd;
    int booked_ssd;                 // 0 = available, 1 = booked
    struct Appointment_ssd* next_ssd;
} Appointment_ssd, *AppointmentPtr_ssd;

// Function prototypes
AppointmentPtr_ssd createSchedule_ssd(int start_ssd, int end_ssd, int duration_ssd);
void display_ssd(AppointmentPtr_ssd head_ssd);
void book_ssd(AppointmentPtr_ssd head_ssd);
void cancel_ssd(AppointmentPtr_ssd head_ssd);
void sortByData_ssd(AppointmentPtr_ssd head_ssd);
AppointmentPtr_ssd sortByPointer_ssd(AppointmentPtr_ssd head_ssd);
void swapData_ssd(AppointmentPtr_ssd a_ssd, AppointmentPtr_ssd b_ssd);

// ---------------- MAIN -----------------
int main() {
    srand((unsigned)time(NULL));

    AppointmentPtr_ssd head_ssd = NULL;
    int start_ssd = 9, end_ssd = 17, duration_ssd = 1; // Working hours: 9 AM to 5 PM (1-hour slots)
    head_ssd = createSchedule_ssd(start_ssd, end_ssd, duration_ssd);

    int choice_ssd;
    while (1) {
        cout << "\n========== Appointment Scheduler ==========\n";
        cout << "1. Display All Slots\n";
        cout << "2. Book Appointment\n";
        cout << "3. Cancel Appointment\n";
        cout << "4. Sort by Time (Swap Data)\n";
        cout << "5. Sort by Time (Pointer Manipulation)\n";
        cout << "6. Exit\n";
        cout << "Enter your choice: ";
        if (!(cin >> choice_ssd)) {
            cout << "Invalid input!\n";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            continue;
        }

        switch (choice_ssd) {
            case 1:
                display_ssd(head_ssd);
                break;
            case 2:
                book_ssd(head_ssd);
                break;
            case 3:
                cancel_ssd(head_ssd);
                break;
            case 4:
                sortByData_ssd(head_ssd);
                cout << "Sorted by swapping data.\n";
                break;
            case 5:
                head_ssd = sortByPointer_ssd(head_ssd);
                cout << "Sorted by pointer manipulation.\n";
                break;
            case 6:
                cout << "Goodbye!\n";
                // free list
                while (head_ssd) {
                    AppointmentPtr_ssd tmp_ssd = head_ssd->next_ssd;
                    delete head_ssd;
                    head_ssd = tmp_ssd;
                }
                exit(0);
            default:
                cout << "Invalid choice!\n";
        }
    }
    return 0;
}

// ---------------- FUNCTIONS -----------------

// Create linked list schedule
AppointmentPtr_ssd createSchedule_ssd(int start_ssd, int end_ssd, int duration_ssd) {
    AppointmentPtr_ssd head_ssd = NULL, temp_ssd = NULL;

    for (int t_ssd = start_ssd; t_ssd < end_ssd; t_ssd += duration_ssd) {
        AppointmentPtr_ssd node_ssd = new Appointment_ssd;
        node_ssd->startHour_ssd = t_ssd;
        node_ssd->endHour_ssd = t_ssd + duration_ssd;
        node_ssd->booked_ssd = rand() % 2; // randomly mark booked or available
        node_ssd->next_ssd = NULL;

        if (head_ssd == NULL)
            head_ssd = node_ssd;
        else
            temp_ssd->next_ssd = node_ssd;
        temp_ssd = node_ssd;
    }
    return head_ssd;
}

// Display schedule
void display_ssd(AppointmentPtr_ssd head_ssd) {
    cout << "\n------ Today's Schedule ------\n";
    AppointmentPtr_ssd temp_ssd = head_ssd;
    while (temp_ssd != NULL) {
        cout << (temp_ssd->startHour_ssd < 10 ? " " : "")
             << temp_ssd->startHour_ssd << ":00 - "
             << (temp_ssd->endHour_ssd < 10 ? " " : "")
             << temp_ssd->endHour_ssd << ":00  =>  "
             << (temp_ssd->booked_ssd ? "Booked" : "Available") << '\n';
        temp_ssd = temp_ssd->next_ssd;
    }
}

// Book appointment
void book_ssd(AppointmentPtr_ssd head_ssd) {
    int start_ssd;
    cout << "Enter start time to book (e.g., 9 for 9:00 AM): ";
    if (!(cin >> start_ssd)) {
        cout << "Invalid input!\n";
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        return;
    }
    AppointmentPtr_ssd temp_ssd = head_ssd;

    while (temp_ssd != NULL) {
        if (temp_ssd->startHour_ssd == start_ssd) {
            if (temp_ssd->booked_ssd == 0) {
                temp_ssd->booked_ssd = 1;
                cout << "Appointment booked successfully!\n";
            } else {
                cout << "Slot already booked!\n";
            }
            return;
        }
        temp_ssd = temp_ssd->next_ssd;
    }
    cout << "Invalid time entered!\n";
}

// Cancel appointment
void cancel_ssd(AppointmentPtr_ssd head_ssd) {
    int start_ssd;
    cout << "Enter start time to cancel: ";
    if (!(cin >> start_ssd)) {
        cout << "Invalid input!\n";
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        return;
    }
    AppointmentPtr_ssd temp_ssd = head_ssd;

    while (temp_ssd != NULL) {
        if (temp_ssd->startHour_ssd == start_ssd) {
            if (temp_ssd->booked_ssd == 1) {
                temp_ssd->booked_ssd = 0;
                cout << "Appointment canceled successfully.\n";
            } else {
                cout << "Slot not booked yet.\n";
            }
            return;
        }
        temp_ssd = temp_ssd->next_ssd;
    }
    cout << "Invalid time entered!\n";
}

// Swap data between two appointments
void swapData_ssd(AppointmentPtr_ssd a_ssd, AppointmentPtr_ssd b_ssd) {
    int tempStart_ssd = a_ssd->startHour_ssd;
    int tempEnd_ssd = a_ssd->endHour_ssd;
    int tempBook_ssd = a_ssd->booked_ssd;

    a_ssd->startHour_ssd = b_ssd->startHour_ssd;
    a_ssd->endHour_ssd = b_ssd->endHour_ssd;
    a_ssd->booked_ssd = b_ssd->booked_ssd;

    b_ssd->startHour_ssd = tempStart_ssd;
    b_ssd->endHour_ssd = tempEnd_ssd;
    b_ssd->booked_ssd = tempBook_ssd;
}

// Sort by swapping data (bubble sort)
void sortByData_ssd(AppointmentPtr_ssd head_ssd) {
    for (AppointmentPtr_ssd i_ssd = head_ssd; i_ssd != NULL; i_ssd = i_ssd->next_ssd) {
        for (AppointmentPtr_ssd j_ssd = i_ssd->next_ssd; j_ssd != NULL; j_ssd = j_ssd->next_ssd) {
            if (i_ssd->startHour_ssd > j_ssd->startHour_ssd)
                swapData_ssd(i_ssd, j_ssd);
        }
    }
}

// Sort by pointer manipulation (insertion sort into new sorted list)
AppointmentPtr_ssd sortByPointer_ssd(AppointmentPtr_ssd head_ssd) {
    if (!head_ssd || !head_ssd->next_ssd)
        return head_ssd;

    AppointmentPtr_ssd sorted_ssd = NULL;
    AppointmentPtr_ssd current_ssd = head_ssd;

    while (current_ssd != NULL) {
        AppointmentPtr_ssd nextNode_ssd = current_ssd->next_ssd;

        if (sorted_ssd == NULL || current_ssd->startHour_ssd < sorted_ssd->startHour_ssd) {
            current_ssd->next_ssd = sorted_ssd;
            sorted_ssd = current_ssd;
        } else {
            AppointmentPtr_ssd temp_ssd = sorted_ssd;
            while (temp_ssd->next_ssd != NULL &&
                   temp_ssd->next_ssd->startHour_ssd < current_ssd->startHour_ssd)
                temp_ssd = temp_ssd->next_ssd;

            current_ssd->next_ssd = temp_ssd->next_ssd;
            temp_ssd->next_ssd = current_ssd;
        }
        current_ssd = nextNode_ssd;
    }
    return sorted_ssd;
}
```

---

## 🧪 Example Execution

```text
========== Appointment Scheduler ==========
1. Display All Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Swap Data)
5. Sort by Time (Pointer Manipulation)
6. Exit
Enter your choice: 1

------ Today's Schedule ------
 9:00 - 10:00  =>  Booked
10:00 - 11:00  =>  Available
11:00 - 12:00  =>  Available
12:00 - 13:00  =>  Booked
13:00 - 14:00  =>  Booked
14:00 - 15:00  =>  Available
15:00 - 16:00  =>  Booked
16:00 - 17:00  =>  Booked

========== Appointment Scheduler ==========
Enter your choice: 2
Enter start time to book (e.g., 9 for 9:00 AM): 9
Slot already booked!

========== Appointment Scheduler ==========
Enter your choice: 2
Enter start time to book (e.g., 9 for 9:00 AM): 10
Appointment booked successfully!

========== Appointment Scheduler ==========
Enter your choice: 2
Enter start time to book (e.g., 9 for 9:00 AM): 11
Appointment booked successfully!

========== Appointment Scheduler ==========
Enter your choice: 4
Sorted by swapping data.

========== Appointment Scheduler ==========
Enter your choice: 1

------ Today's Schedule ------
 9:00 - 10:00  =>  Booked
10:00 - 11:00  =>  Booked
11:00 - 12:00  =>  Booked
12:00 - 13:00  =>  Booked
13:00 - 14:00  =>  Booked
14:00 - 15:00  =>  Available
15:00 - 16:00  =>  Booked
16:00 - 17:00  =>  Booked

========== Appointment Scheduler ==========
Enter your choice: 5
Sorted by pointer manipulation.

========== Appointment Scheduler ==========
Enter your choice: 1

------ Today's Schedule ------
 9:00 - 10:00  =>  Booked
10:00 - 11:00  =>  Booked
11:00 - 12:00  =>  Booked
12:00 - 13:00  =>  Booked
13:00 - 14:00  =>  Booked
14:00 - 15:00  =>  Available
15:00 - 16:00  =>  Booked
16:00 - 17:00  =>  Booked

========== Appointment Scheduler ==========
Enter your choice: 3
Enter start time to cancel: 9
Appointment canceled successfully.

========== Appointment Scheduler ==========
Enter your choice: 1

------ Today's Schedule ------
 9:00 - 10:00  =>  Available
10:00 - 11:00  =>  Booked
11:00 - 12:00  =>  Booked
12:00 - 13:00  =>  Booked
13:00 - 14:00  =>  Booked
14:00 - 15:00  =>  Available
15:00 - 16:00  =>  Booked
16:00 - 17:00  =>  Booked

========== Appointment Scheduler ==========
Enter your choice: 6
Goodbye!
```

---

## 🧠 Memory Visualization

### Node Structure (Appointment Slot)

Each node in the singly linked list:

| Field             | Description                              |
|-------------------|------------------------------------------|
| `startHour_ssd`   | Slot starting time (e.g., 9, 10, 11)     |
| `endHour_ssd`     | Slot ending time (e.g., 10, 11, 12)      |
| `booked_ssd`      | `0` = Available, `1` = Booked            |
| `next_ssd`        | Pointer to the next appointment node     |

### Linked List Layout (Example)

```text
[9-10, Booked] -> [10-11, Booked] -> [11-12, Booked] ->
[12-13, Booked] -> [13-14, Booked] -> [14-15, Available] ->
[15-16, Booked] -> [16-17, Booked] -> NULL
```

- After pointer-based sort, **only links change**, not the actual data inside nodes.

---

## ✅ Conclusion

This assignment demonstrates:

- Use of **singly linked lists** for time-based scheduling.  
- Implementation of:
  - **Random initialization** of availability.
  - **Booking** and **cancellation** with validation.
  - Two sorting techniques:
    - **By data swapping** (Bubble Sort).
    - **By pointer manipulation** (Insertion-like Sort without swapping data).
- Proper input validation and memory cleanup.

This provides a strong understanding of **linked lists**, **sorting via pointers**, and practical modeling of **real-world systems** like appointment schedulers.

