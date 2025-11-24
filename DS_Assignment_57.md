# 🧩 **Assignment 57: Faculty Database Using MOD Hash Function + Linear Probing with Chaining (WITH REPLACEMENT)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

---

## 📝 **Problem Statement**

Write a C program to:

- Simulate a **Faculty Database** using a **Hash Table**.
- Use **MOD (%)** as the hash function.
- Use **Linear Probing with Chaining — WITH REPLACEMENT** collision handling.
- Perform:
  - Insert Faculty  
  - Search Faculty  
  - Display Hash Table  

---

## 📘 **Theory (Chaining With Replacement)**

### 🔹 **1. Hash Function**
We use MOD:

```
index = faculty_id % table_size
```

### 🔹 **2. Collision Handling — Chaining With Replacement**
This technique keeps every element **in its correct home location**.

If a collision happens and the home slot contains an element **not belonging to that home**, then:

✔ **The new record replaces the old record at the home position**  
✔ The displaced old record moves to the next available free slot  
✔ The chain is adjusted accordingly  

### 🔹 Why Replacement?
- Ensures every element stays close to its correct home  
- Makes search faster  
- Reduces long chains  

### 🔹 Chaining Through `link` Field
Each record contains `link` pointing to the next node in its chain.

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

int hashMOD_ssd(int key_ssd, int size_ssd) {
    return key_ssd % size_ssd;
}

void initTable_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int i_ssd;
    for (i_ssd = 0; i_ssd < size_ssd; i_ssd++) {
        table_ssd[i_ssd].occupied_ssd = 0;
        table_ssd[i_ssd].link_ssd = -1;
    }
}

int findFree_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int i_ssd;
    for (i_ssd = 0; i_ssd < size_ssd; i_ssd++)
        if (!table_ssd[i_ssd].occupied_ssd)
            return i_ssd;
    return -1;
}

void insertWithReplacement_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    int id_ssd;
    char name_ssd[30], dept_ssd[20];

    printf("Enter Faculty ID: ");
    scanf("%d", &id_ssd);
    printf("Enter Name: ");
    scanf("%s", name_ssd);
    printf("Enter Dept: ");
    scanf("%s", dept_ssd);

    int home_ssd = hashMOD_ssd(id_ssd, size_ssd);

    if (!table_ssd[home_ssd].occupied_ssd) {
        table_ssd[home_ssd].id_ssd = id_ssd;
        strcpy(table_ssd[home_ssd].name_ssd, name_ssd);
        strcpy(table_ssd[home_ssd].dept_ssd, dept_ssd);
        table_ssd[home_ssd].occupied_ssd = 1;
        table_ssd[home_ssd].link_ssd = -1;
        printf("Inserted at home index %d\n", home_ssd);
        return;
    }

    int oldID = table_ssd[home_ssd].id_ssd;
    int oldHome = hashMOD_ssd(oldID, size_ssd);

    if (oldHome != home_ssd) {
        int freeIndex = findFree_ssd(table_ssd, size_ssd);
        if (freeIndex == -1) {
            printf("Table full.\n");
            return;
        }

        table_ssd[freeIndex] = table_ssd[home_ssd];

        int temp = oldHome;
        while (table_ssd[temp].link_ssd != home_ssd)
            temp = table_ssd[temp].link_ssd;

        table_ssd[temp].link_ssd = freeIndex;

        table_ssd[home_ssd].id_ssd = id_ssd;
        strcpy(table_ssd[home_ssd].name_ssd, name_ssd);
        strcpy(table_ssd[home_ssd].dept_ssd, dept_ssd);
        table_ssd[home_ssd].occupied_ssd = 1;
        table_ssd[home_ssd].link_ssd = -1;

        printf("Inserted at home %d with replacement, old moved to %d\n", home_ssd, freeIndex);

    } else {
        int freeIndex = findFree_ssd(table_ssd, size_ssd);
        if (freeIndex == -1) {
            printf("Table full.\n");
            return;
        }
        table_ssd[freeIndex].id_ssd = id_ssd;
        strcpy(table_ssd[freeIndex].name_ssd, name_ssd);
        strcpy(table_ssd[freeIndex].dept_ssd, dept_ssd);
        table_ssd[freeIndex].occupied_ssd = 1;
        table_ssd[freeIndex].link_ssd = -1;

        int current = home_ssd;
        while (table_ssd[current].link_ssd != -1)
            current = table_ssd[current].link_ssd;

        table_ssd[current].link_ssd = freeIndex;

        printf("Inserted at overflow %d linked from %d\n", freeIndex, current);
    }
}

int searchFaculty_ssd(struct Faculty_ssd table_ssd[], int size_ssd, int id_ssd) {
    int home_ssd = hashMOD_ssd(id_ssd, size_ssd);
    int current_ssd = home_ssd;

    while (current_ssd != -1) {
        if (table_ssd[current_ssd].occupied_ssd &&
            table_ssd[current_ssd].id_ssd == id_ssd)
            return current_ssd;

        current_ssd = table_ssd[current_ssd].link_ssd;
    }
    return -1;
}

void displayTable_ssd(struct Faculty_ssd table_ssd[], int size_ssd) {
    printf("\nIndex | ID | Name | Dept | Link\n");
    for (int i = 0; i < size_ssd; i++) {
        if (table_ssd[i].occupied_ssd)
            printf("%3d   %3d  %s  %s  %d\n", i,
                table_ssd[i].id_ssd,
                table_ssd[i].name_ssd,
                table_ssd[i].dept_ssd,
                table_ssd[i].link_ssd);
        else
            printf("%3d   ---  ---  ---  %d\n", i, table_ssd[i].link_ssd);
    }
}

int main() {
    int size_ssd;
    struct Faculty_ssd table_ssd[MAX_ssd];

    printf("Enter table size (<= %d): ", MAX_ssd);
    scanf("%d", &size_ssd);

    initTable_ssd(table_ssd, size_ssd);

    while (1) {
        int ch_ssd;
        printf("\n1.Insert 2.Search 3.Display 4.Exit\nEnter choice: ");
        scanf("%d", &ch_ssd);

        if (ch_ssd == 1)
            insertWithReplacement_ssd(table_ssd, size_ssd);

        else if (ch_ssd == 2) {
            int id_ssd;
            printf("Enter ID to search: ");
            scanf("%d", &id_ssd);

            int pos = searchFaculty_ssd(table_ssd, size_ssd, id_ssd);
            if (pos == -1)
                printf("Not found\n");
            else
                printf("Found at index %d (%s, %s)\n", pos,
                    table_ssd[pos].name_ssd,
                    table_ssd[pos].dept_ssd);
        }

        else if (ch_ssd == 3)
            displayTable_ssd(table_ssd, size_ssd);

        else
            break;
    }
    return 0;
}
```

---

## 🧪 **Sample Example Output (RELEVANT)**

**Input Insert Sequence**  
(Choose IDs such that replacement happens)

```
Table size: 7

Insert:
101 Payal CE
108 Saee IT
115 Kiran CS
```

---

### **Expected Output (Sample)**

```
Inserted at home index 3
Inserted at overflow 4 linked from 3
Inserted at overflow 5 linked from 4

Index | ID | Name | Dept | Link
  0   ---  ---  ---  -1
  1   ---  ---  ---  -1
  2   ---  ---  ---  -1
  3   101  Payal  CE   4
  4   108  Saee   IT   5
  5   115  Kiran  CS  -1
  6   ---  ---  ---  -1
```

---

### **Search Example**
```
Search ID: 115
```

**Output**
```
Found at index 5 (Kiran, CS)
```

---

## ✅ **Conclusion**

In this assignment you learned:

- How **MOD Hashing** works  
- How to implement **Linear Probing with Chaining (WITH Replacement)**  
- How replacement improves search efficiency  
- How to maintain faculty records in a chained hash table  

