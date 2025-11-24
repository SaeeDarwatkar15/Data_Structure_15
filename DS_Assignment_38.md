# 🧩 Assignment 38: **Employee Search and Sorting Using BST (Tree Data Structure)**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**

This assignment uses a **Binary Search Tree (BST)** to:
- **Efficiently search** an employee record by `empId`
- **Sort** and display all employee records in **ascending order of empId** using **inorder traversal**

## 📝 Problem Statement
Write a C++ program that:
- Stores employee records (`empId`, `name`, `dept`, `salary`) in a **BST**
- Allows the user to:
  - Insert new employees
  - Search for an employee using **Employee ID**
  - Display all employees sorted by **Employee ID** (ascending)
- Uses tree traversal to provide **sorted output** without using any extra sorting algorithm.

## 📘 Theory

### Why BST for Employee Records?
A **Binary Search Tree** organizes data so that:
- Left subtree contains nodes with **smaller keys**
- Right subtree contains nodes with **larger keys**

Here, the **key** is `empId_ssd`.  
This structure allows:
- **Efficient search**: O(log n) average time
- **Natural sorting**: Inorder traversal gives data in **ascending** key order

### Operations Used
- **Insert**: Place new employee in proper position based on `empId_ssd`
- **Search**: Traverse left/right depending on comparison with `empId_ssd`
- **Inorder Traversal**: Visit `Left → Node → Right` to get **sorted employees**

## 💻 CODE 

```cpp
#include <iostream>
using namespace std;

struct Employee_ssd {
    int empId_ssd;
    string name_ssd, dept_ssd;
    float salary_ssd;
    Employee_ssd *left_ssd, *right_ssd;

    Employee_ssd(int id_ssd, string n_ssd, string d_ssd, float s_ssd) {
        empId_ssd = id_ssd;
        name_ssd = n_ssd;
        dept_ssd = d_ssd;
        salary_ssd = s_ssd;
        left_ssd = right_ssd = nullptr;
    }
};

// INSERT in BST based on empId
Employee_ssd* insert_ssd(Employee_ssd* root_ssd, Employee_ssd* node_ssd) {
    if (!root_ssd)
        return node_ssd;

    if (node_ssd->empId_ssd < root_ssd->empId_ssd)
        root_ssd->left_ssd = insert_ssd(root_ssd->left_ssd, node_ssd);
    else
        root_ssd->right_ssd = insert_ssd(root_ssd->right_ssd, node_ssd);

    return root_ssd;
}

// SEARCH using empId
Employee_ssd* search_ssd(Employee_ssd* root_ssd, int id_ssd) {
    if (!root_ssd || root_ssd->empId_ssd == id_ssd)
        return root_ssd;

    if (id_ssd < root_ssd->empId_ssd)
        return search_ssd(root_ssd->left_ssd, id_ssd);

    return search_ssd(root_ssd->right_ssd, id_ssd);
}

// INORDER DISPLAY (sorted by empId)
void inorder_ssd(Employee_ssd* root_ssd) {
    if (!root_ssd) return;

    inorder_ssd(root_ssd->left_ssd);
    cout << "ID: " << root_ssd->empId_ssd
         << " | Name: " << root_ssd->name_ssd
         << " | Dept: " << root_ssd->dept_ssd
         << " | Salary: " << root_ssd->salary_ssd << "\n";
    inorder_ssd(root_ssd->right_ssd);
}

// MAIN MENU
int main() {
    Employee_ssd* root_ssd = nullptr;
    int choice_ssd;

    while (true) {
        cout << "\n=== Employee Tree Menu (ssd) ===\n";
        cout << "1. Insert Employee\n";
        cout << "2. Search Employee by ID\n";
        cout << "3. Display All Employees Sorted\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice_ssd;

        if (choice_ssd == 1) {
            int id_ssd;
            string name_ssd, dept_ssd;
            float salary_ssd;

            cout << "Enter Employee ID: ";
            cin >> id_ssd;
            cout << "Enter Name: ";
            cin >> name_ssd;
            cout << "Enter Department: ";
            cin >> dept_ssd;
            cout << "Enter Salary: ";
            cin >> salary_ssd;

            Employee_ssd* node_ssd = new Employee_ssd(id_ssd, name_ssd, dept_ssd, salary_ssd);
            root_ssd = insert_ssd(root_ssd, node_ssd);
            cout << "Employee inserted successfully!\n";
        }
        else if (choice_ssd == 2) {
            int id_ssd;
            cout << "Enter Employee ID to search: ";
            cin >> id_ssd;

            Employee_ssd* result_ssd = search_ssd(root_ssd, id_ssd);
            if (result_ssd) {
                cout << "\nEmployee Found:\n";
                cout << "ID: " << result_ssd->empId_ssd
                     << " | Name: " << result_ssd->name_ssd
                     << " | Dept: " << result_ssd->dept_ssd
                     << " | Salary: " << result_ssd->salary_ssd << "\n";
            } else {
                cout << "Employee NOT found.\n";
            }
        }
        else if (choice_ssd == 3) {
            cout << "\nEmployees Sorted by ID:\n";
            inorder_ssd(root_ssd);
        }
        else if (choice_ssd == 4) {
            cout << "Exiting...\n";
            break;
        }
        else {
            cout << "Invalid choice! Try again.\n";
        }
    }
    return 0;
}
```


## 🧮 Example Execution

Input Employees:
ID = 103  Name = Riya   Dept = HR    Salary = 40000
ID = 101  Name = Amit   Dept = IT    Salary = 55000
ID = 110  Name = Karan  Dept = Admin Salary = 45000

Sorted Output (Inorder on empId):
ID: 101 | Name: Amit  | Dept: IT    | Salary: 55000
ID: 103 | Name: Riya  | Dept: HR    | Salary: 40000
ID: 110 | Name: Karan | Dept: Admin | Salary: 45000

Search Example:
Enter Employee ID to search: 103
Employee Found:
ID: 103 | Name: Riya | Dept: HR | Salary: 40000


## 🧠 Memory Visualization

Conceptual Tree (by empId):

        103 (Riya)
       /        \
  101 (Amit)   110 (Karan)

Inorder (Left → Node → Right) gives:
101 (Amit), 103 (Riya), 110 (Karan) → sorted by **Employee ID**.


## ✅ Conclusion
This program shows how a **Binary Search Tree** can be used to:
- **Efficiently search** employee records by `empId`  
- **Automatically keep records sorted** without extra sorting algorithms  

Key learnings:
- How **insertion by key** organizes employees in a BST  
- How **search** follows left/right branches based on `empId` comparison  
- How **inorder traversal** prints employees in **ascending order of empId**

This assignment strengthens understanding of **BST-based indexing**, **record searching**, and **sorted traversal** for real-world applications like employee databases.
