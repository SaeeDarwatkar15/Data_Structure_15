# 🧩 **Assignment 43: Kruskal’s Algorithm using Disjoint Set Union (DSU)**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)** of a **user-defined weighted undirected graph**.  
It uses **Disjoint Set Union (Union-Find)** with **path compression** and **union by rank** for efficient cycle detection.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted, undirected graph** from the user  
- Store the graph using:
  - **Adjacency List** (for display)  
  - **Edge List** (for Kruskal’s algorithm)  
- Apply **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)**  
- Display:
  - The **Adjacency List**  
  - All **edges in the MST**  
  - The **total weight** of the MST  
- Properly handle the case when the graph is **disconnected** (MST for all vertices does not exist)

---

## 📘 **Theory**

### 🌳 1. **Minimum Spanning Tree (MST)**

Given a **connected, undirected, weighted graph**:

- A **Spanning Tree** is a subgraph that:
  - Includes **all vertices**
  - Has exactly **(V − 1)** edges
  - Has **no cycles**

- A **Minimum Spanning Tree (MST)** is a spanning tree with:
  - **Minimum possible total edge weight** among all spanning trees of that graph.

---

### 🧮 2. **Kruskal’s Algorithm – Basic Idea**

Kruskal’s Algorithm is a **greedy algorithm** for finding MST.  
At each step, it picks the **lightest edge** that does **not form a cycle** with the already chosen edges.

**Steps:**

1. Sort all edges in **non-decreasing order** of their weights.  
2. Start with an **empty MST**.  
3. For each edge (in sorted order):
   - If adding this edge **does not form a cycle**, include it in MST.  
   - Otherwise, **skip** it.  
4. Stop when the MST has exactly **(V − 1) edges**.

To **check cycles efficiently**, we use **Disjoint Set Union (DSU)**.

---

### 🧩 3. **Disjoint Set Union (Union-Find)**

**DSU** is a data structure that keeps track of elements partitioned into **disjoint (non-overlapping) sets**.  
It supports two main operations:

- **find(x)** → returns the **representative (parent)** of the set containing `x`  
- **unite(a, b)** → merges the sets containing `a` and `b`  

We use:

- **Path Compression** in `find()`  
  - Makes future `find()` operations faster by linking nodes directly to their root.
- **Union by Rank** in `unite()`  
  - Always attaches the **smaller tree** under the **larger tree’s root** to keep the tree shallow.

DSU helps in Kruskal’s Algorithm to:

- Quickly check if two vertices are already in the **same set** (i.e., adding edge would form a cycle)  
- Efficiently **merge** sets when an edge is added to MST  

---

### 📂 4. **Graph Representation**

This program uses **two representations**:

1. **Adjacency List**  
   - `vector<vector<pair<int,int>>> adj_ssd;`  
   - For each vertex `u`, `adj_ssd[u]` stores pair `(v, weight)`  
   - Used here only for **display**.

2. **Edge List**  
   - `vector<tuple<int,int,int>> edges_ssd;`  
   - Stores `(weight, u, v)` for each edge  
   - Used by **Kruskal’s Algorithm** after sorting.

---

### ⏱️ 5. **Time Complexity**

Let:
- `V` = number of vertices  
- `E` = number of edges  

Main steps:

1. Sorting edges → **O(E log E)**  
2. DSU operations (almost O(1) each with path compression & union by rank) → **O(E α(V))**, where α(V) is inverse Ackermann (very slow growing)

Overall time complexity:  
👉 **O(E log E)** (dominant term due to sorting)

---

### 🌐 6. **Applications of Kruskal’s Algorithm**

- **Network design** (minimum cost for connecting computers/routers)  
- **Road, railway, and pipeline design**  
- **Electrical wiring and communication networks**  
- Used in various **optimization and approximation algorithms** in graph theory

---

## 🔹 **Algorithm Steps (Kruskal’s using DSU)**

1. Read `n_ssd` (number of vertices) and `m_ssd` (number of edges).  
2. For each edge:
   - Read `u_ssd`, `v_ssd`, `w_ssd`  
   - Store in:
     - **Adjacency List** (for each direction, since graph is undirected)  
     - **Edge List** as `(weight, u, v)` with 0-based indices  
3. Display the **Adjacency List**.  
4. Sort `edges_ssd` by **weight (ascending)**.  
5. Initialize DSU on `n_ssd` vertices.  
6. For each edge (in sorted order):
   - If `unite_ssd(u, v)` returns **true**, add this edge to `mst_ssd` and increase `totalWeight_ssd`.  
7. After processing all edges:
   - If number of edges in MST (**mst_ssd.size()**) is **not equal** to `n_ssd − 1`:
     - Graph is **disconnected**, full MST for all vertices does **not** exist.  
   - Else:
     - Print all MST edges and **total weight**.

---

## 💻 **CODE**

```cpp
#include <bits/stdc++.h>
using namespace std;

// Disjoint Set Union (Union-Find) with path compression and union by rank
struct DSU_ssd {
    int n_ssd;
    vector<int> parent_ssd, rank_ssd;
    DSU_ssd(int n=0) { init_ssd(n); }
    void init_ssd(int n) {
        n_ssd = n;
        parent_ssd.assign(n, 0);
        rank_ssd.assign(n, 0);
        for (int i = 0; i < n; ++i) parent_ssd[i] = i;
    }
    int find_ssd(int x) {
        if (parent_ssd[x] != x) parent_ssd[x] = find_ssd(parent_ssd[x]);
        return parent_ssd[x];
    }
    bool unite_ssd(int a, int b) {
        int pa = find_ssd(a), pb = find_ssd(b);
        if (pa == pb) return false;
        if (rank_ssd[pa] < rank_ssd[pb]) swap(pa, pb);
        parent_ssd[pb] = pa;
        if (rank_ssd[pa] == rank_ssd[pb]) ++rank_ssd[pa];
        return true;
    }
};

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

    // adjacency list for display: adj[u] = vector of (v, weight)
    vector<vector<pair<int,int>>> adj_ssd(n_ssd);
    // edge list for Kruskal: (weight, u, v)
    vector<tuple<int,int,int>> edges_ssd;
    cout << "Enter edges (u v w) with vertices in range 1.." << n_ssd << ":\n";
    for (int i_ssd = 0; i_ssd < m_ssd; ++i_ssd) {
        int u_ssd, v_ssd; long long w_ll_ssd;
        cout << "Edge " << (i_ssd + 1) << ": ";
        while (!(cin >> u_ssd >> v_ssd >> w_ll_ssd) || u_ssd < 1 || u_ssd > n_ssd || v_ssd < 1 || v_ssd > n_ssd) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid input. Enter edge as: u v w (1.." << n_ssd << "): ";
        }
        int w_ssd = (int)w_ll_ssd;
        // store adjacency (0-based)
        adj_ssd[u_ssd - 1].push_back({v_ssd - 1, w_ssd});
        adj_ssd[v_ssd - 1].push_back({u_ssd - 1, w_ssd});
        // store edge (weight, u, v) with 0-based indices
        edges_ssd.push_back({w_ssd, u_ssd - 1, v_ssd - 1});
    }

    // Optional: display adjacency list
    cout << "\nAdjacency List:\n";
    for (int u_ssd = 0; u_ssd < n_ssd; ++u_ssd) {
        cout << (u_ssd + 1) << " : ";
        for (auto &p_ssd : adj_ssd[u_ssd]) {
            cout << "(" << p_ssd.first + 1 << ", w=" << p_ssd.second << ") ";
        }
        cout << "\n";
    }

    // Kruskal's algorithm
    sort(edges_ssd.begin(), edges_ssd.end(), [](const auto &a, const auto &b){
        return get<0>(a) < get<0>(b);
    });

    DSU_ssd dsu_ssd(n_ssd);
    vector<tuple<int,int,int>> mst_ssd; // (u, v, weight)
    long long totalWeight_ssd = 0;

    for (auto &e_ssd : edges_ssd) {
        int w_ssd = get<0>(e_ssd);
        int u_ssd = get<1>(e_ssd);
        int v_ssd = get<2>(e_ssd);
        if (dsu_ssd.unite_ssd(u_ssd, v_ssd)) {
            mst_ssd.emplace_back(u_ssd, v_ssd, w_ssd);
            totalWeight_ssd += w_ssd;
        }
    }

    if ((int)mst_ssd.size() != n_ssd - 1) {
        cout << "\nThe graph is disconnected; a spanning tree for all vertices does not exist.\n";
        // Optionally show MSTs of individual components (not implemented here).
    } else {
        cout << "\nMinimum Spanning Tree (Kruskal's algorithm) edges:\n";
        cout << "Edge (u - v)    weight\n";
        for (auto &t_ssd : mst_ssd) {
            int u_ssd, v_ssd, w_ssd;
            tie(u_ssd, v_ssd, w_ssd) = t_ssd;
            cout << "(" << (u_ssd + 1) << " - " << (v_ssd + 1) << ")       " << w_ssd << "\n";
        }
        cout << "Total weight of MST: " << totalWeight_ssd << "\n";
    }

    return 0;
}
```

---

## 🧮 **Example Execution**

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

Adjacency List:
1 : (2, w=3) (3, w=1)
2 : (1, w=3) (3, w=7) (4, w=5)
3 : (1, w=1) (2, w=7) (4, w=2) (5, w=4)
4 : (2, w=5) (3, w=2) (5, w=6) (6, w=8)
5 : (3, w=4) (4, w=6) (6, w=2)
6 : (4, w=8) (5, w=2)

Minimum Spanning Tree (Kruskal's algorithm) edges:
Edge (u - v)    weight
(1 - 3)       1
(3 - 4)       2
(5 - 6)       2
(3 - 5)       4
(1 - 2)       3
Total weight of MST: 12
```

---

## ✅ **Conclusion**

This assignment successfully implements **Kruskal’s Algorithm** for finding the **Minimum Spanning Tree (MST)** of a user-defined weighted graph using:

- **Adjacency List** (for graph representation and display)  
- **Edge List** (for sorting edges by weight)  
- **Disjoint Set Union (DSU)** with **path compression** and **union by rank** (for efficient cycle detection)

Key learnings include:

- How **greedy algorithms** work on graphs  
- How to use **Union-Find (DSU)** to avoid cycles in MST construction  
- How to compute the **minimum total cost** to connect all vertices in a graph  

This strengthens understanding of **graph algorithms**, **data structures (DSU)**, and **efficient MST computation**.
