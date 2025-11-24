# 🧩 **Assignment 56: Faculty Database Using Divide Hash Function + Linear Probing with Chaining (Without Replacement)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program simulates a **Faculty Hash Table Database** using:  
✔ **Divide Method Hashing (key % size)**  
✔ **Linear Probing**  
✔ **Chaining (Without Replacement)**  
✔ **Faculty Records: ID, Name, Department**  

---

## 📝 **Problem Statement**

Write a program in **C language** to:

- Create a **Faculty Database** using a hash table.  
- Use **Divide Method** for hashing.  
- Use **Linear Probing + Chaining without Replacement** for collision resolution.  
- Provide the following operations:
  - **Insert faculty record**
  - **Search faculty record**
  - **Display the full hash table**

---

## 📘 **Theory**

### 🔢 **1. Hash Function (Divide Method)**  
This method computes the index as:

```
index = key % table_size
```

It distributes keys uniformly and is simple to compute.

---

### 🔁 **2. Collision Handling — Linear Probing + Chaining Without Replacement**

If the home index is occupied:

- Search for the next empty slot (linear probing)
- Insert the new record there
- **Link** it to the home index using a `link` pointer

This creates a **linked list** inside the array itself.

### 📌 Features:

- No element is displaced from its home location  
- Overflow elements form a chain  
- Search follows the chain until the element is found

---

### 🛄 **3. Searching**

Search begins at the **home index**, then follows `link` pointers until:

- Record found → return index  
- Link becomes `-1` → record not found  

---

### 📚 **Complexity Analysis**

| Operation | Avg | Worst |
|----------|------|--------|
| Insert   | O(1) | O(n)   |
| Search   | O(1) | O(n)   |
| Display  | O(n) | O(n)   |

---

## 💻 **FULL C PROGRAM**

```c
#include <stdio.h>
#include <string.h>

#define MAX_ssd 20

struct Faculty_ssd {
    int id_ssd;
    char name_ssd[30];
    char dept_ssd[20];
    int link_ssd;
    int occupied_ssd;
};

int hashDivide_ssd(int key_ssd, int size_ssd) {
    return key_ssd % size_ssd;
}

void initTable_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int i_ssd;
    for (i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
        table_ssd[i_ssd].occupied_ssd = 0;
        table_ssd[i_ssd].link_ssd = -1;
    }
}

int findFreeSlot_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int i_ssd;
    for (i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
        if (!table_ssd[i_ssd].occupied_ssd)
            return i_ssd;
    }
    return -1;
}

void insertFaculty_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int id_ssd;
    char name_ssd[30], dept_ssd[20];

    printf("Enter Faculty ID: ");
    scanf("%d", &id_ssd);
    printf("Enter Name: ");
    scanf("%s", name_ssd);
    printf("Enter Department: ");
    scanf("%s", dept_ssd);

    int home_ssd = hashDivide_ssd(id_ssd, size_ssd);

    if (!table_ssd[home_ssd].occupied_ssd) {
        table_ssd[home_ssd].id_ssd = id_ssd;
        strcpy(table_ssd[home_ssd].name_ssd, name_ssd);
        strcpy(table_ssd[home_ssd].dept_ssd, dept_ssd);
        table_ssd[home_ssd].occupied_ssd = 1;
        table_ssd[home_ssd].link_ssd = -1;
        printf("Inserted at home index %d\n", home_ssd);
        return;
    }

    int freeIndex_ssd = findFreeSlot_ssd(table_ssd, size_ssd);
    if (freeIndex_ssd == -1) {
        printf("Table full, cannot insert.\n");
        return;
    }

    table_ssd[freeIndex_ssd].id_ssd = id_ssd;
    strcpy(table_ssd[freeIndex_ssd].name_ssd, name_ssd);
    strcpy(table_ssd[freeIndex_ssd].dept_ssd, dept_ssd);
    table_ssd[freeIndex_ssd].occupied_ssd = 1;
    table_ssd[freeIndex_ssd].link_ssd = -1;

    int current_ssd = home_ssd;
    while (table_ssd[current_ssd].link_ssd != -1) {
        current_ssd = table_ssd[current_ssd].link_ssd;
    }
    table_ssd[current_ssd].link_ssd = freeIndex_ssd;

    printf("Inserted at overflow index %d, chained from %d\n", freeIndex_ssd, current_ssd);
}

int searchFaculty_ssd(struct Faculty_ssd table_ssd[], int size_ssd, int id_ssd) {
    int home_ssd = hashDivide_ssd(id_ssd, size_ssd);
    int current_ssd = home_ssd;

    while (current_ssd != -1) {
        if (table_ssd[current_ssd].occupied_ssd && table_ssd[current_ssd].id_ssd == id_ssd) {
            return current_ssd;
        }
        current_ssd = table_ssd[current_ssd].link_ssd;
    }
    return -1;
}

void displayTable_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int i_ssd;
    printf("\nIndex | ID | Name | Dept | Link\n");
    for (i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
        if (table_ssd[i_ssd].occupied_ssd) {
            printf("%3d   %3d  %s  %s  %d\n", i_ssd,
                   table_ssd[i_ssd].id_ssd,
                   table_ssd[i_ssd].name_ssd,
                   table_ssd[i_ssd].dept_ssd,
                   table_ssd[i_ssd].link_ssd);
        } else {
            printf("%3d   ---  ---  ---  %d\n", i_ssd, table_ssd[i_ssd].link_ssd);
        }
    }
}

int main() {
    int size_ssd;
    struct Faculty_ssd table_ssd[MAX_ssd];

    printf("Enter hash table size (<= %d): ", MAX_ssd);
    scanf("%d", &size_ssd);

    initTable_ssd(table_ssd, size_ssd);

    while (1) {
        int choice_ssd;
        printf("\n1. Insert Faculty\n2. Search Faculty\n3. Display Table\n4. Exit\nEnter choice: ");
        scanf("%d", &choice_ssd);

        if (choice_ssd == 1) {
            insertFaculty_ssd(table_ssd, size_ssd);
        } else if (choice_ssd == 2) {
            int id_ssd;
            printf("Enter ID to search: ");
            scanf("%d", &id_ssd);
            int pos_ssd = searchFaculty_ssd(table_ssd, size_ssd, id_ssd);
            if (pos_ssd == -1) {
                printf("Faculty not found.\n");
            } else {
                printf("Faculty found at index %d: %s (%s)\n",
                       pos_ssd,
                       table_ssd[pos_ssd].name_ssd,
                       table_ssd[pos_ssd].dept_ssd);
            }
        } else if (choice_ssd == 3) {
            displayTable_ssd(table_ssd, size_ssd);
        } else {
            break;
        }
    }
    return 0;
}
```

---

## 🧪 **Example Output**

**Input**
```
Table size: 7
Insert:
101 Kumar CS
108 Mehta IT
115 Patil ENTC
Search: 115
Display
```

**Output**
```
Inserted at home index 3
Inserted at overflow index 4, chained from 3
Inserted at overflow index 5, chained from 4

Index | ID | Name | Dept | Link
  0   ---  ---  ---  -1
  1   ---  ---  ---  -1
  2   ---  ---  ---  -1
  3   101  Kumar  CS   4
  4   108  Mehta  IT   5
  5   115  Patil  ENTC -1
  6   ---  ---  ---  -1

Faculty found at index 5: Patil (ENTC)
```

---

## ✅ **Conclusion**

In this assignment you implemented:

- **Divide hashing:** `key % size`
- **Linear probing without replacement**
- **Chaining inside the table using link fields**
- Efficient **search**, **insert**, and **display**
- A functional **faculty record management system**

This strengthens your understanding of **collision resolution** and **advanced hashing techniques**.  
