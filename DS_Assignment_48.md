# 🧩 **Assignment 48: Prim’s Algorithm using Adjacency List**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Prim’s Algorithm** to find the **Minimum Spanning Tree (MST)** of a **user-defined weighted undirected graph**.  
The graph is represented using an **Adjacency List**, and a **Min-Heap (Priority Queue)** is used to always pick the **minimum weight edge** expanding the MST.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted, undirected graph** from the user  
- Represent the graph using an **Adjacency List**  
- Apply **Prim’s Algorithm** to find the **Minimum Spanning Tree (MST)**  
- Display:
  - All **edges** selected in the MST  
  - The **total weight (cost)** of the MST  

---

## 📘 **Theory**

### 🌳 1. **Minimum Spanning Tree (MST)**

For a **connected, undirected, weighted graph**:

- A **Spanning Tree** is a subgraph that:
  - Includes **all vertices**
  - Has exactly **(V − 1)** edges
  - Has **no cycles**

- A **Minimum Spanning Tree (MST)** is a spanning tree whose **sum of edge weights** is **minimum** among all possible spanning trees of that graph.

---

### 🧮 2. **Prim’s Algorithm – Greedy Strategy**

Prim’s Algorithm builds the MST **vertex by vertex**:

1. Start from any **initial vertex** (here, vertex `0`).  
2. Maintain a set of vertices **already in MST**.  
3. At each step, choose the **minimum weight edge** that:
   - Connects a vertex **inside the MST**  
   - To a vertex **outside the MST**  
4. Add that edge and the new vertex to the MST.  
5. Repeat until all vertices are included.

We use:

- `key_ssd[v]` → the **minimum weight** edge that connects vertex `v` to the growing MST  
- `parent_ssd[v]` → the vertex from which `v` is connected in the MST  
- `inMST_ssd[v]` → whether vertex `v` is already in MST  
- A **min-heap (priority_queue with greater)** to always pick the vertex with the **smallest key value**

---

### 📂 3. **Adjacency List Representation**

The graph is stored as:

```cpp
vector<vector<pair<int,int>>> adj_ssd;
```

For each vertex `u_ssd`:

- `adj_ssd[u_ssd]` contains pairs `(v_ssd, wt_ssd)` representing:
  - `v_ssd` = neighbor vertex  
  - `wt_ssd` = weight of edge `(u_ssd, v_ssd)`

Since the graph is **undirected**, each input edge `(u, v, w)` is stored as:

- `adj_ssd[u].push_back({v, w});`  
- `adj_ssd[v].push_back({u, w});`

---

### ⏱️ 4. **Time Complexity**

Let:

- `V` = number of vertices  
- `E` = number of edges  

Using:

- **Adjacency List**  
- **Min-Heap priority queue**  

Prim’s Algorithm runs in:  
👉 **O(E log V)**

This is efficient for **sparse graphs**.

---

### 🌐 5. **Applications**

Prim’s Algorithm and MST are used in:

- **Network design** (cables, telephone lines, internet wiring)  
- **Road and railway connection planning**  
- **Electrical grid optimization**  
- Any application needing **minimum cost connection** of all nodes.

---

## 💻 **CODE**

```cpp

#include <bits/stdc++.h>
using namespace std;
using pii = pair<int,int>;

// Function to perform Prim's algorithm and print MST
void prim_ssd(int V_ssd, vector<vector<pair<int,int>>>& adj_ssd) {
    vector<int> inMST_ssd(V_ssd, 0);
    vector<int> key_ssd(V_ssd, INT_MAX);
    vector<int> parent_ssd(V_ssd, -1);

    // Min-heap (priority queue) storing (key, vertex)
    priority_queue<pii, vector<pii>, greater<pii>> pq_ssd;

    int start_ssd = 0;           // Start from vertex 0
    key_ssd[start_ssd] = 0;
    pq_ssd.push({0, start_ssd});

    while (!pq_ssd.empty()) {
        auto [w_ssd, u_ssd] = pq_ssd.top();
        pq_ssd.pop();

        if (inMST_ssd[u_ssd]) {
            continue;            // Skip if already included in MST
        }

        inMST_ssd[u_ssd] = 1;

        // Relax all adjacent vertices
        for (auto &pr_ssd : adj_ssd[u_ssd]) {
            int v_ssd = pr_ssd.first;
            int wt_ssd = pr_ssd.second;

            // If v_ssd not in MST and wt_ssd is smaller than current key
            if (!inMST_ssd[v_ssd] && wt_ssd < key_ssd[v_ssd]) {
                key_ssd[v_ssd] = wt_ssd;
                parent_ssd[v_ssd] = u_ssd;
                pq_ssd.push({key_ssd[v_ssd], v_ssd});
            }
        }
    }

    int total_ssd = 0;
    cout << "Edges in MST:\n";
    for (int i_ssd = 1; i_ssd < V_ssd; i_ssd++) {
        cout << parent_ssd[i_ssd] << " - " << i_ssd << " : " << key_ssd[i_ssd] << "\n";
        total_ssd += key_ssd[i_ssd];
    }
    cout << "Total: " << total_ssd << "\n";
}

int main() {
    int V_ssd;
    cout << "Vertices: ";
    cin >> V_ssd;

    int E_ssd;
    cout << "Edges: ";
    cin >> E_ssd;

    vector<vector<pair<int,int>>> adj_ssd(V_ssd);

    cout << "Enter u v w edges:\n";
    for (int i_ssd = 0; i_ssd < E_ssd; i_ssd++) {
        int u_ssd, v_ssd, w_ssd;
        cin >> u_ssd >> v_ssd >> w_ssd;
        // Undirected graph: add edge in both directions
        adj_ssd[u_ssd].push_back({v_ssd, w_ssd});
        adj_ssd[v_ssd].push_back({u_ssd, w_ssd});
    }

    prim_ssd(V_ssd, adj_ssd);

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input:**

```text
Vertices: 4
Edges: 5
Enter u v w edges:
0 1 1
0 2 4
1 2 2
1 3 6
2 3 3
```

**Output:**

```text
Edges in MST:
0 - 1 : 1
1 - 2 : 2
2 - 3 : 3
Total: 6
```

**Explanation:**

- Graph edges:
  - 0–1 (1), 0–2 (4), 1–2 (2), 1–3 (6), 2–3 (3)
- Prim’s Algorithm starting from vertex `0` chooses:
  - Edge `0 - 1` with weight `1`
  - Edge `1 - 2` with weight `2`
  - Edge `2 - 3` with weight `3`
- Total MST weight: `1 + 2 + 3 = 6`  
- All **4 vertices** are connected with **minimum possible total cost**.

---

## ✅ **Conclusion**

In this assignment, we:

- Represented a **weighted undirected graph** using an **Adjacency List**  
- Implemented **Prim’s Algorithm** using:
  - **key array** to track the minimum edge weight to each vertex  
  - **parent array** to reconstruct the MST  
  - **min-heap priority queue** to always expand using the **lightest edge**  
- Computed and displayed:
  - All **edges** in the **Minimum Spanning Tree (MST)**  
  - The **total weight** of the MST  

This program deepens understanding of:

- **Greedy algorithms** on graphs  
- Efficient MST construction using **Adjacency List + Min-Heap**  
- Practical application of **priority queues** in optimizing graph algorithms.
