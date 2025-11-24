# 🧩 **Assignment 45: Graph Representation using Adjacency List with BFS and DFS Traversals**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program accepts a **user-defined graph**, represents it using an **Adjacency List**, and performs both **BFS (Breadth-First Search)** and **DFS (Depth-First Search)** traversals from a given start vertex. It supports **directed** as well as **undirected** graphs.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a graph from the user  
- Represent the graph using an **Adjacency List**  
- Perform:
  - **Breadth-First Search (BFS)** traversal  
  - **Depth-First Search (DFS)** traversal  
- Allow the user to:
  - **Display the adjacency list**  
  - **Run BFS from any start vertex**  
  - **Run DFS from any start vertex**  
  - **Choose directed or undirected graph**

---

## 📘 **Theory**

### 🌐 1. **Graph and Adjacency List**

A **graph** consists of:

- A set of **vertices (nodes)**  
- A set of **edges**, which connect pairs of vertices  

To represent a graph efficiently, we use an **Adjacency List**:

```cpp
vector<vector<int>> adj_ssd;
```

- For each vertex `u`, `adj_ssd[u]` is a list of all vertices directly connected to `u`.  
- For an **undirected** graph, if there is an edge `(u, v)`, then:
  - `v` is added to the list of `u`  
  - `u` is added to the list of `v`  
- For a **directed** graph, if there is an edge from `u` to `v`, only:
  - `v` is added to the list of `u`

**Advantages of Adjacency List:**

- Saves memory for **sparse graphs** (few edges)  
- Efficient to iterate over neighbors of a vertex  

---

### 🚶‍♀️ 2. **Breadth-First Search (BFS)**

**BFS** is a graph traversal technique that:

- Visits vertices **level by level** (like ripples spreading outwards)  
- Uses a **queue** data structure  

**Steps:**

1. Start from a given **start vertex**.  
2. Mark it as **visited** and push it into the **queue**.  
3. Repeatedly:
   - Pop a vertex `u` from the front of the queue  
   - Visit all **unvisited neighbors** of `u`, mark them visited, and push them into the queue  

**Applications:**

- Finding **shortest path** in unweighted graphs  
- Checking **connected components**  
- Level-order traversal  

**Time Complexity:**  
👉 `O(V + E)` where `V` = vertices, `E` = edges.

---

### 🧗‍♀️ 3. **Depth-First Search (DFS)**

**DFS** is a traversal technique that:

- Goes **as deep as possible** along one branch before backtracking  
- Can be implemented using:
  - **Recursion** or  
  - **Explicit stack**

In this program, DFS is implemented **recursively**.

**Steps:**

1. Start from a given **start vertex**.  
2. Mark it as **visited**, print it.  
3. Recursively visit all **unvisited neighbors**.  

**Applications:**

- Detecting **cycles** in graphs  
- Finding **connected components**  
- Topological sorting (in DAGs)  

**Time Complexity:**  
👉 `O(V + E)`.

---

### 🔗 4. **Menu-Driven Approach**

The program uses a **menu** to let the user:

1. Display the **Adjacency List**  
2. Perform **BFS** from a chosen start vertex  
3. Perform **DFS** from a chosen start vertex  
4. **Exit** the program  

This makes testing and understanding the traversals more interactive.

---

## 📊 **Algorithm Outline**

1. Input number of vertices `V_ssd`.  
2. Initialize `adj_ssd` as a vector of size `V_ssd`.  
3. Input number of edges `E_ssd`.  
4. Ask the user if the graph is **directed** or **undirected** (`0/1`).  
5. For each edge:
   - Input `u_ssd`, `v_ssd`  
   - Call `addEdge_ssd(adj_ssd, u_ssd, v_ssd, directed_ssd)`  
6. Repeatedly show menu:
   - **1. Display adjacency list**  
   - **2. BFS** → Input start vertex and call `bfs_ssd()`  
   - **3. DFS** → Input start vertex and call `dfs_ssd()`  
   - **4. Exit**  

---

## 💻 **CODE**

```cpp
#include <bits/stdc++.h>
using namespace std;

// Add edge to adjacency list
void addEdge_ssd(vector<vector<int>>& adj_ssd, int u_ssd, int v_ssd, bool directed_ssd = false) {
    adj_ssd[u_ssd].push_back(v_ssd);
    if (!directed_ssd) {
        // For undirected graph, add edge both ways
        adj_ssd[v_ssd].push_back(u_ssd);
    }
}

// Display adjacency list
void displayAdj_ssd(const vector<vector<int>>& adj_ssd) {
    cout << "Adjacency List:\n";
    for (int i = 0; i < (int)adj_ssd.size(); ++i) {
        cout << i << ": ";
        for (int v_ssd : adj_ssd[i]) {
            cout << v_ssd << " ";
        }
        cout << "\n";
    }
}

// Breadth-First Search (BFS)
void bfs_ssd(const vector<vector<int>>& adj_ssd, int start_ssd) {
    int V_ssd = adj_ssd.size();
    vector<int> visited_ssd(V_ssd, 0);
    queue<int> q_ssd;

    visited_ssd[start_ssd] = 1;
    q_ssd.push(start_ssd);

    cout << "BFS starting from " << start_ssd << ": ";
    while (!q_ssd.empty()) {
        int u_ssd = q_ssd.front();
        q_ssd.pop();
        cout << u_ssd << " ";

        for (int v_ssd : adj_ssd[u_ssd]) {
            if (!visited_ssd[v_ssd]) {
                visited_ssd[v_ssd] = 1;
                q_ssd.push(v_ssd);
            }
        }
    }
    cout << "\n";
}

// DFS Utility (recursive)
void dfsUtil_ssd(const vector<vector<int>>& adj_ssd, int u_ssd, vector<int>& visited_ssd) {
    visited_ssd[u_ssd] = 1;
    cout << u_ssd << " ";
    for (int v_ssd : adj_ssd[u_ssd]) {
        if (!visited_ssd[v_ssd]) {
            dfsUtil_ssd(adj_ssd, v_ssd, visited_ssd);
        }
    }
}

// Depth-First Search (DFS)
void dfs_ssd(const vector<vector<int>>& adj_ssd, int start_ssd) {
    vector<int> visited_ssd(adj_ssd.size(), 0);
    cout << "DFS starting from " << start_ssd << ": ";
    dfsUtil_ssd(adj_ssd, start_ssd, visited_ssd);
    cout << "\n";
}

int main() {
    int V_ssd;
    cout << "Enter vertices: ";
    cin >> V_ssd;

    vector<vector<int>> adj_ssd(V_ssd);

    int E_ssd;
    cout << "Enter edges: ";
    cin >> E_ssd;

    cout << "Directed? (0/1): ";
    int d_ssd;
    cin >> d_ssd;
    bool directed_ssd = (d_ssd == 1);

    cout << "Enter edges (u v):\n";
    for (int i_ssd = 0; i_ssd < E_ssd; i_ssd++) {
        int u_ssd, v_ssd;
        cin >> u_ssd >> v_ssd;
        addEdge_ssd(adj_ssd, u_ssd, v_ssd, directed_ssd);
    }

    while (true) {
        cout << "\n1.Display Adj List  2.BFS  3.DFS  4.Exit\nChoice: ";
        int ch_ssd;
        cin >> ch_ssd;

        if (ch_ssd == 1) {
            displayAdj_ssd(adj_ssd);
        } 
        else if (ch_ssd == 2) {
            int s_ssd;
            cout << "Start vertex for BFS: ";
            cin >> s_ssd;
            bfs_ssd(adj_ssd, s_ssd);
        } 
        else if (ch_ssd == 3) {
            int s_ssd;
            cout << "Start vertex for DFS: ";
            cin >> s_ssd;
            dfs_ssd(adj_ssd, s_ssd);
        } 
        else if (ch_ssd == 4) {
            cout << "Exiting...\n";
            break;
        } 
        else {
            cout << "Invalid choice. Try again.\n";
        }
    }

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input / Output (Sample Run)**

```text
Enter vertices: 5
Enter edges: 5
Directed? (0/1): 0
Enter edges (u v):
0 1
0 2
1 2
1 3
3 4

1.Display Adj List  2.BFS  3.DFS  4.Exit
Choice: 1
Adjacency List:
0: 1 2 
1: 0 2 3 
2: 0 1 
3: 1 4 
4: 3 

1.Display Adj List  2.BFS  3.DFS  4.Exit
Choice: 2
Start vertex for BFS: 0
BFS starting from 0: 0 1 2 3 4 

1.Display Adj List  2.BFS  3.DFS  4.Exit
Choice: 3
Start vertex for DFS: 0
DFS starting from 0: 0 1 2 3 4 

1.Display Adj List  2.BFS  3.DFS  4.Exit
Choice: 4
Exiting...
```

---

## ✅ **Conclusion**

In this assignment, we:

- Accepted a **user-defined graph**
- Represented it using an **Adjacency List**
- Implemented and executed both:
  - **Breadth-First Search (BFS)** using a **queue**  
  - **Depth-First Search (DFS)** using **recursion**  

This program helps to clearly understand:

- How to **store graphs efficiently** using adjacency lists  
- The difference between **BFS (level-wise traversal)** and **DFS (depth-wise traversal)**  
- Fundamental graph traversal techniques, which are the base for many advanced graph algorithms like **shortest paths, connectivity, cycle detection, and spanning trees**.
