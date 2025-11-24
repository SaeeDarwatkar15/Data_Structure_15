# 🧩 **Assignment 42: Prim’s Algorithm using Adjacency List**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Prim’s Algorithm** to find the **Minimum Spanning Tree (MST)** of a **user-defined weighted graph** using an **Adjacency List** and **Min-Heap (Priority Queue)**. It takes graph input from the user and prints the **MST edges** and their **total minimum cost**.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted, undirected graph** from the user  
- Represent the graph using an **Adjacency List**  
- Apply **Prim’s Algorithm** to find the **Minimum Spanning Tree (MST)**  
- Display:
  - All edges selected in the MST  
  - The **total weight** of the MST  
- Handle invalid input and disconnected graphs properly  

---

## 📘 **Theory**

### 🌳 1. **Graph and Minimum Spanning Tree (MST)**

A **graph** consists of:
- **Vertices (nodes)**  
- **Edges (connections)** with associated **weights (costs)**  

A **Spanning Tree** of a connected, undirected graph is:
- A subgraph that:
  - Includes **all vertices**
  - Has exactly **(V - 1)** edges
  - Contains **no cycles**

A **Minimum Spanning Tree (MST)** is a spanning tree with:
- **Minimum possible total edge weight** among all spanning trees of that graph.

---

### ⚙️ 2. **Prim’s Algorithm – Idea**

Prim’s Algorithm is a **greedy algorithm** for finding MST:

1. Start with **any one vertex** (here we start from vertex `1`).  
2. At each step, select the **edge with the smallest weight** that:  
   - Connects a **vertex already in MST**  
   - To a **vertex not yet in MST**  
3. Add that edge and the new vertex to the MST.  
4. Repeat until **all vertices** are included.

This **greedy choice** at each step ensures the resulting tree has **minimum total weight**.

---

### 📂 3. **Adjacency List Representation**

For a graph with `V` vertices:

- We use a vector:  
  `vector<vector<pair<int, int>>> adj_ssd;`  
  where for each vertex `u`:  
  - `adj_ssd[u]` is a list of `(v, w)` pairs  
  - `v` = neighbor vertex  
  - `w` = weight of edge `(u, v)`

**Advantages of Adjacency List:**

- Saves memory for **sparse graphs** (few edges compared to `V²`)  
- Efficient traversal of all neighbors of a vertex  

---

### 📦 4. **Min-Heap (Priority Queue)**

We use a **min-heap** (priority queue with `greater` comparator) that stores tuples:  
`(weight, vertex, parent)`.

At each step:
- We extract the **tuple with smallest weight**  
- That represents the **lightest edge** to a new vertex  
- This helps to efficiently choose the next edge to add to MST

---

### 🧮 5. **Key Arrays Used**

- `key_ssd[]`  
  - `key_ssd[v]` = current **minimum weight** edge that can connect vertex `v` to MST  
  - Initialized to `INT_MAX`  
- `parent_ssd[]`  
  - Stores the **parent (previous) vertex** of each vertex in the MST  
- `inMST_ssd[]`  
  - Boolean array that tells whether a vertex is **already included** in MST  

---

### ⏱️ 6. **Time Complexity**

Let:
- `V` = number of vertices  
- `E` = number of edges  

Using:
- **Adjacency List** + **Min-Heap (Priority Queue)**  

The time complexity of Prim’s Algorithm is:  
👉 **O(E log V)**

This is efficient for large, sparse graphs.

---

### 🌐 7. **Applications of MST**

Prim’s Algorithm and MST are widely used in:

- **Network design** (minimum cost for laying cables, telephone lines, Internet links)  
- **Road and railway connections** with minimum cost  
- **Electrical grid design**  
- **Approximation algorithms** for complex graph problems  

---

## 🔹 **Step-by-Step Algorithm (Prim’s using Adjacency List)**

1. Input **number of vertices** (`n_ssd`) and **edges** (`m_ssd`).  
2. Create an **adjacency list** `adj_ssd` of size `n_ssd`.  
3. For each edge:
   - Read `u_ssd`, `v_ssd`, `w_ssd`
   - Store in adjacency list for both directions (because graph is undirected).  
4. Initialize:
   - `key_ssd` with `INT_MAX`  
   - `inMST_ssd` with `false`  
   - `parent_ssd` with `-1`  
5. Choose **start vertex** as `0` (which is vertex `1` in user input).  
6. Push `(0, 0, -1)` into min-heap.  
7. While min-heap is not empty:
   - Extract vertex `u_ssd` with minimum edge weight `w_ssd`  
   - If `u_ssd` already in MST → continue  
   - Mark `u_ssd` as in MST  
   - If `parent_ssd[u_ssd] != -1`, add edge `(parent_ssd[u_ssd], u_ssd)` to MST  
   - For each neighbor `v_ssd` of `u_ssd`:
     - If `v_ssd` **not in MST** and `wt_ssd < key_ssd[v_ssd]`:
       - Update `key_ssd[v_ssd]`  
       - Push `(wt_ssd, v_ssd, u_ssd)` into min-heap  
8. After loop, check if **all vertices** are in MST:  
   - If any vertex is not visited → graph is **disconnected**, MST does **not** exist for whole graph  
9. Otherwise:
   - Print all **MST edges (u, v, weight)**  
   - Print **total weight** of MST  

---

## 💻 **CODE**

```cpp
#include <bits/stdc++.h>
using namespace std;

// Prim's algorithm using adjacency list and min-heap
// All variables/functions suffixed with _ssd

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n_ssd;
    cout << "Enter number of vertices: ";
    while (!(cin >> n_ssd) || n_ssd <= 0) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Please enter a positive integer for vertices: ";
    }

    int m_ssd;
    cout << "Enter number of edges: ";
    while (!(cin >> m_ssd) || m_ssd < 0) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Please enter a non-negative integer for edges: ";
    }

    // adjacency list: adj[u] contains pairs (v, weight)
    vector<vector<pair<int,int>>> adj_ssd(n_ssd);
    cout << "Enter edges (u v w) with vertices in range 1.." << n_ssd << ":\n";
    for (int i_ssd = 0; i_ssd < m_ssd; ++i_ssd) {
        int u_ssd, v_ssd;
        long long w_ssd; // read weight as long long but store as int
        cout << "Edge " << (i_ssd + 1) << ": ";
        while (!(cin >> u_ssd >> v_ssd >> w_ssd) ||
               u_ssd < 1 || u_ssd > n_ssd ||
               v_ssd < 1 || v_ssd > n_ssd) {

            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid input. Enter edge as: u v w (1.." << n_ssd << "): ";
        }
        // convert to 0-based indices
        adj_ssd[u_ssd - 1].push_back({v_ssd - 1, (int)w_ssd});
        adj_ssd[v_ssd - 1].push_back({u_ssd - 1, (int)w_ssd});
    }

    // Min-heap of (weight, vertex, parent)
    using T_ssd = tuple<int,int,int>; // (weight, vertex, parent)
    priority_queue<T_ssd, vector<T_ssd>, greater<T_ssd>> pq_ssd;

    vector<bool> inMST_ssd(n_ssd, false);
    vector<int> parent_ssd(n_ssd, -1);
    vector<int> key_ssd(n_ssd, INT_MAX);

    // Start from vertex 0 (which is vertex 1 for the user)
    key_ssd[0] = 0;
    pq_ssd.push({0, 0, -1});

    long long totalWeight_ssd = 0;
    vector<tuple<int,int,int>> mstEdges_ssd; // (u, v, weight)

    while (!pq_ssd.empty()) {
        auto [w_ssd, u_ssd, p_ssd] = pq_ssd.top();
        pq_ssd.pop();

        if (inMST_ssd[u_ssd])
            continue; // already included

        // include u in MST
        inMST_ssd[u_ssd] = true;
        parent_ssd[u_ssd] = p_ssd;
        key_ssd[u_ssd] = w_ssd;

        if (p_ssd != -1) {
            // record edge (p_ssd <-> u_ssd) with weight w_ssd
            mstEdges_ssd.push_back({p_ssd, u_ssd, w_ssd});
            totalWeight_ssd += w_ssd;
        }

        // relax adjacent edges
        for (auto &edge_ssd : adj_ssd[u_ssd]) {
            int v_ssd = edge_ssd.first;
            int wt_ssd = edge_ssd.second;
            if (!inMST_ssd[v_ssd] && wt_ssd < key_ssd[v_ssd]) {
                key_ssd[v_ssd] = wt_ssd;
                pq_ssd.push({wt_ssd, v_ssd, u_ssd});
            }
        }
    }

    // Check if MST includes all vertices
    bool connected_ssd = true;
    for (int i_ssd = 0; i_ssd < n_ssd; ++i_ssd) {
        if (!inMST_ssd[i_ssd]) {
            connected_ssd = false;
            break;
        }
    }

    if (!connected_ssd) {
        cout << "\nThe graph is disconnected; Minimum Spanning Tree does not exist for the whole graph.\n";
    } else {
        cout << "\nMinimum Spanning Tree (Prim's algorithm) edges:\n";
        cout << "Edge (u - v)   weight\n";
        for (auto &t_ssd : mstEdges_ssd) {
            int a_ssd, b_ssd, wt_ssd;
            tie(a_ssd, b_ssd, wt_ssd) = t_ssd;
            // convert back to 1-based for display
            cout << "(" << (a_ssd + 1) << " - " << (b_ssd + 1) << ")     " << wt_ssd << "\n";
        }
        cout << "Total weight of MST: " << totalWeight_ssd << "\n";
    }

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input / Output (Sample Run)**

```text
Enter number of vertices: 6
Enter number of edges: 9
Enter edges (u v w) with vertices in range 1..6:
Edge 1: 1 2 3
Edge 2: 1 3 1
Edge 3: 2 3 7
Edge 4: 2 4 5
Edge 5: 3 4 2
Edge 6: 3 5 4
Edge 7: 4 5 6
Edge 8: 4 6 8
Edge 9: 5 6 2

Minimum Spanning Tree (Prim's algorithm) edges:
Edge (u - v)   weight
(1 - 3)     1
(3 - 4)     2
(5 - 6)     2
(3 - 5)     4
(1 - 2)     3
Total weight of MST: 12
```

---

## 🧠 **Concept Visualization (Simple View)**

The MST built by Prim’s Algorithm for the above input can be thought of like:

```text
1 --1-- 3 --2-- 4
|       |
3       4
 \      |
  2     5 --2-- 6
```

**Selected MST edges:**
- (1 – 3) weight 1  
- (3 – 4) weight 2  
- (5 – 6) weight 2  
- (3 – 5) weight 4  
- (1 – 2) weight 3  

Total weight = **1 + 2 + 2 + 4 + 3 = 12**

---

## ✅ **Conclusion**

In this assignment, we implemented **Prim’s Algorithm** using:

- **Adjacency List** to store graph efficiently  
- **Priority Queue (Min-Heap)** to always pick the minimum weight edge  
- **Key, Parent, and Visited arrays** to manage MST formation  

This program:

- Builds a **Minimum Spanning Tree (MST)** from a user-defined graph  
- Ensures the **minimum total cost** to connect all vertices  
- Demonstrates how **greedy algorithms** work on graphs  
- Strengthens understanding of **graph representation**, **heap usage**, and **optimal tree construction**.
