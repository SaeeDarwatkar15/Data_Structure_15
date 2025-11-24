# 🧩 Assignment 26: **Clinic Management System Using Queue and Multiple Doctors**

**Author:** **Saee Darwatkar**  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** **Data Structures**  

This assignment demonstrates how to manage a medical clinic using **Queue (FIFO)** for patient check-ins and **multiple doctors** for assignment based on **first-come, first-served** scheduling.

## 📝 Problem Statement
Design a program that:
- Keeps track of **patients** as they check into a clinic  
- Uses a **queue** to store patients on a **first-come, first-served** basis  
- Assigns patients to **available doctors**  
- Allows doctors to **finish** with patients and automatically take the next one  
- Displays waiting queue and doctor status  

## 📘 Theory

### 🔹 Queue (FIFO)
A **queue** is a linear data structure that follows **FIFO (First In, First Out)**.  
This is ideal for patient management because:
- The first patient to arrive should be served first  
- Preserves fairness and real-life clinic scenario  

### 🔹 Why Queue for Patients?
- Ensures **ordered serving**  
- Prevents skipping  
- Suitable for scheduling, OS process handling, and simulations  

### 🔹 Doctor Management
Each doctor has:
- **Name**  
- **Busy/Free status**  
- **Current patient**  

Doctor operations:
- Accept next patient  
- Finish and become free  
- Auto-assign next waiting patient  

### 🔹 Data Structures Used
- **queue<string>** → waiting patients  
- **vector<Doctor_ssd>** → multiple doctors  
- **struct Doctor_ssd** → hold doctor details  

### 🔹 Applications
- Hospital & clinic management  
- Ticket counters, customer service systems  
- Job scheduling in Operating Systems  
- Real-time waiting list systems  

## 🔹 Key Concepts
- **Push** → patient enters queue  
- **Pop** → patient assigned to doctor  
- **Queue Front** → person to serve next  
- **Multiple servers (doctors)** → resource management  
- **Automatic allocation** → improves efficiency  

## 📊 Algorithm
1. Enter number of doctors and their names  
2. Create an empty patient queue  
3. Show menu repeatedly  
4. Options:
   - **Check-in:** Add patient to queue  
   - **Assign one:** Give next patient to selected doctor  
   - **Auto-assign:** Assign all free doctors automatically  
   - **Finish:** Doctor completes appointment → becomes free → auto-assign next patient  
   - **Display:** Show waiting queue and doctor status  
5. Continue until exit  
6. End program  

## 💻 CODE

```cpp
    #include <iostream>
    #include <queue>
    #include <vector>
    #include <string>
    #include <limits>

    using namespace std;

    struct Doctor_ssd {
        string name_ssd;
        bool busy_ssd;
        string patient_ssd;
        Doctor_ssd(const string &n = "") : name_ssd(n), busy_ssd(false), patient_ssd("") {}
    };

    void showMenu_ssd() {
        cout << "\n--- Clinic Management ---\n";
        cout << "1. Check-in patient (add to waiting queue)\n";
        cout << "2. Serve next patient by doctor\n";
        cout << "3. Auto-assign waiting patients to all free doctors\n";
        cout << "4. Finish with doctor\n";
        cout << "5. Display status\n";
        cout << "6. Exit\n";
        cout << "Enter choice: ";
    }

    void readLine_ssd(string &out_ssd) {
        getline(cin, out_ssd);
        if (out_ssd.size() == 0) getline(cin, out_ssd);
    }

    int main() {
        int numDoctors_ssd;
        cout << "Enter number of doctors in clinic: ";
        while (!(cin >> numDoctors_ssd) || numDoctors_ssd <= 0) {
            cout << "Please enter a positive integer for doctors: ";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        vector<Doctor_ssd> doctors_ssd;
        doctors_ssd.reserve(numDoctors_ssd);
        for (int i = 0; i < numDoctors_ssd; ++i) {
            string dname_ssd;
            cout << "Enter name or id for doctor " << (i + 1) << ": ";
            readLine_ssd(dname_ssd);
            if (dname_ssd.empty()) dname_ssd = string("Dr") + to_string(i + 1);
            doctors_ssd.emplace_back(dname_ssd);
        }

        queue<string> waiting_ssd;
        int choice_ssd;

        while (true) {
            showMenu_ssd();
            if (!(cin >> choice_ssd)) {
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Invalid input. Try again.\n";
                continue;
            }
            cin.ignore(numeric_limits<streamsize>::max(), '\n');

            if (choice_ssd == 1) {
                string pname_ssd;
                cout << "Enter patient name: ";
                readLine_ssd(pname_ssd);
                if (pname_ssd.empty()) cout << "Patient name cannot be empty.\n";
                else {
                    waiting_ssd.push(pname_ssd);
                    cout << "Patient '" << pname_ssd << "' checked in.\n";
                }
            }
            else if (choice_ssd == 2) {
                if (waiting_ssd.empty()) {
                    cout << "No patients waiting.\n";
                    continue;
                }
                cout << "Choose doctor:\n";
                for (int i = 0; i < doctors_ssd.size(); ++i)
                    cout << (i + 1) << ". " << doctors_ssd[i].name_ssd 
                         << (doctors_ssd[i].busy_ssd ? " (Busy)" : " (Free)") << "\n";

                int didx_ssd;
                cout << "Enter doctor number: ";
                if (!(cin >> didx_ssd) || didx_ssd < 1 || didx_ssd > doctors_ssd.size()) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid doctor number.\n";
                    continue;
                }
                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                Doctor_ssd &doc_ssd = doctors_ssd[didx_ssd - 1];
                if (doc_ssd.busy_ssd)
                    cout << doc_ssd.name_ssd << " is busy with '" << doc_ssd.patient_ssd << "'.\n";
                else {
                    string nextPatient_ssd = waiting_ssd.front();
                    waiting_ssd.pop();
                    doc_ssd.patient_ssd = nextPatient_ssd;
                    doc_ssd.busy_ssd = true;
                    cout << doc_ssd.name_ssd << " is now seeing '" << nextPatient_ssd << "'.\n";
                }
            }
            else if (choice_ssd == 3) {
                bool assigned = false;
                for (int i = 0; i < doctors_ssd.size() && !waiting_ssd.empty(); ++i) {
                    if (!doctors_ssd[i].busy_ssd) {
                        string nextPatient_ssd = waiting_ssd.front();
                        waiting_ssd.pop();
                        doctors_ssd[i].patient_ssd = nextPatient_ssd;
                        doctors_ssd[i].busy_ssd = true;
                        cout << doctors_ssd[i].name_ssd << " assigned '" << nextPatient_ssd << "'.\n";
                        assigned = true;
                    }
                }
                if (!assigned) cout << "No free doctors or no patients.\n";
            }
            else if (choice_ssd == 4) {
                cout << "Choose doctor:\n";
                for (int i = 0; i < doctors_ssd.size(); ++i)
                    cout << (i + 1) << ". " << doctors_ssd[i].name_ssd 
                         << (doctors_ssd[i].busy_ssd ? " (Busy)" : " (Free)") << "\n";

                int didx_ssd;
                cout << "Enter doctor number: ";
                if (!(cin >> didx_ssd) || didx_ssd < 1 || didx_ssd > doctors_ssd.size()) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Invalid choice.\n";
                    continue;
                }
                cin.ignore(numeric_limits<streamsize>::max(), '\n');

                Doctor_ssd &doc_ssd = doctors_ssd[didx_ssd - 1];
                if (!doc_ssd.busy_ssd)
                    cout << doc_ssd.name_ssd << " is already free.\n";
                else {
                    cout << doc_ssd.name_ssd << " finished with '" << doc_ssd.patient_ssd << "'.\n";
                    doc_ssd.patient_ssd.clear();
                    doc_ssd.busy_ssd = false;

                    if (!waiting_ssd.empty()) {
                        string next = waiting_ssd.front(); waiting_ssd.pop();
                        doc_ssd.patient_ssd = next;
                        doc_ssd.busy_ssd = true;
                        cout << "Automatically assigned '" << next << "'.\n";
                    }
                }
            }
            else if (choice_ssd == 5) {
                cout << "\nWaiting queue: ";
                if (waiting_ssd.empty()) cout << "(empty)\n";
                else {
                    queue<string> tmp = waiting_ssd;
                    while (!tmp.empty()) {
                        cout << "'" << tmp.front() << "'";
                        tmp.pop();
                        if (!tmp.empty()) cout << ", ";
                    }
                    cout << "\n";
                }

                cout << "\nDoctors status:\n";
                for (auto &doc : doctors_ssd) {
                    cout << doc.name_ssd << " : "
                         << (doc.busy_ssd ? "Busy with '" + doc.patient_ssd + "'" : "Free") << "\n";
                }
            }
            else if (choice_ssd == 6) {
                cout << "Exiting...\n";
                break;
            }
            else cout << "Invalid choice.\n";
        }

        return 0;
    }
```


## 🧠 System Visualization

### **Patient Flow**
- Patients enter → stored in **queue**:  
  `Front → [Jay, Vijay, Ajay] → Back`  
- First patient is always served first  

### **Doctor Flow**
| Doctor | Status | Current Patient |
|--------|---------|-----------------|
| Dr. Anisha | Busy | Jay → Ajay |
| Dr. Soham | Busy | Vijay |

### **Automatic Assignment**
When doctor finishes:
- Takes next patient automatically  
- Ensures **no idle doctor** while queue is not empty  


## 🧮 Example Execution 
Enter number of doctors: **2**  
Doctors: **Dr. Anisha**, **Dr. Soham**

Patients check-in:
- Jay  
- Vijay  
- Ajay  

Auto-assign:
- Dr. Anisha → Jay  
- Dr. Soham → Vijay  

Finish:
- Dr. Anisha finishes Jay → auto-assign Ajay  

Final state:
- Waiting queue: empty  
- Doctors:  
  - Dr. Anisha: Ajay  
  - Dr. Soham: Vijay  

## ✅ Conclusion
This assignment demonstrates how **queue-based scheduling** and **multi-server (doctor) management** can be implemented using basic data structures.  
It reinforces:
- FIFO queue concept  
- Real-time resource allocation  
- Handling multiple service providers  
- Practical application of **vector**, **queue**, and **struct** in C++  

It closely models a real-life clinic system and strengthens data structure implementation skills.
