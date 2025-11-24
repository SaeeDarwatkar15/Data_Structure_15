# 🧩 **Assignment 49: Kruskal’s Algorithm using Adjacency List / Edge List**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)** of a **user-defined weighted undirected graph**.  
The graph is represented using an **edge list** (which is a simple form of adjacency representation), and **Disjoint Set Union (DSU / Union-Find)** is used to efficiently avoid cycles while building the MST.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted, undirected graph** from the user  
- Represent the graph using a **list of edges (u, v, w)**  
- Apply **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)**  
- Display:
  - All **edges** selected in the MST  
  - The **total weight** of the MST  

Assume:

- Vertices are numbered from **0 to V − 1**  
- All **edge weights are positive**

---

## 📘 **Theory**

### 🌳 1. **Minimum Spanning Tree (MST)**

For a **connected, undirected, weighted graph**:

- A **Spanning Tree**:
  - Includes **all vertices**  
  - Has exactly **(V − 1)** edges  
  - Contains **no cycles**

- A **Minimum Spanning Tree (MST)** is a spanning tree whose **total edge weight** is **minimum** among all possible spanning trees.

---

### 🧮 2. **Kruskal’s Algorithm – Greedy Strategy**

Kruskal’s Algorithm builds the MST by **sorting edges by weight**:

1. Collect all edges `(u, v, w)` of the graph.  
2. Sort all edges in **non-decreasing order** of weights `w`.  
3. Initialize MST as an **empty set of edges**.  
4. For each edge (from smallest to largest weight):
   - If adding this edge **does not form a cycle**, include it in the MST.  
   - Otherwise, **skip** the edge.  
5. Stop when MST contains **(V − 1)** edges.

To check whether including an edge forms a **cycle**, we use **Disjoint Set Union (DSU / Union-Find)**.

---

### 🧩 3. **Disjoint Set Union (DSU / Union-Find)**

DSU manages a partition of vertices into **disjoint sets**.

Operations:

- `find_ssd(x)`  
  - Finds the **representative (root)** of the set containing vertex `x`.  
  - Uses **path compression** to make future operations faster.

- `unite_ssd(a, b)`  
  - Merges the sets containing vertices `a` and `b`.  
  - Uses **union by rank** to keep trees shallow and operations efficient.

**In Kruskal’s Algorithm:**

- If `find_ssd(u) != find_ssd(v)` → `u` and `v` are in **different components** → adding edge `(u, v)` will **not form a cycle**, so we **include** it in MST and unite the sets.  
- Otherwise, the edge would form a cycle and is **skipped**.

---

### 📂 4. **Edge List Representation**

The program uses a **simple edge list**:

```cpp
vector<tuple<int,int,int>> edges_ssd; // (weight, u, v)
```

For each input edge `(u_ssd, v_ssd, w_ssd)`:

- We store it as `edges_ssd.emplace_back(w_ssd, u_ssd, v_ssd);`

This structure is perfect for Kruskal’s Algorithm because:

- It makes **sorting by weight** very easy.  
- We don’t need adjacency information; we only need edges with their weights.

---

### ⏱️ 5. **Time Complexity**

Let:

- `V` = number of vertices  
- `E` = number of edges  

Main steps:

1. Sorting edges: **O(E log E)**  
2. DSU operations on edges: almost **O(E)** (amortized, due to path compression + union by rank)

So the overall time complexity is:  
👉 **O(E log E)**

---

### 🌐 6. **Applications**

Kruskal’s MST algorithm is used in:

- **Network design** (minimum cost cabling/connection)  
- **Road, railway, and pipeline design**  
- **Clustering algorithms**  
- Any application requiring **minimum cost connection** across all nodes.

---

## 💻 **CODE**

```cpp

#include <bits/stdc++.h>
using namespace std;

// Disjoint Set Union (Union-Find) structure
struct DSU_ssd {
    vector<int> p, r;

    DSU_ssd(int n) {
        p.resize(n);
        r.assign(n, 0);
        for (int i = 0; i < n; i++) {
            p[i] = i;           // each vertex is its own parent initially
        }
    }

    // Find with path compression
    int find_ssd(int x) {
        return p[x] == x ? x : p[x] = find_ssd(p[x]);
    }

    // Union by rank
    void unite_ssd(int a, int b) {
        a = find_ssd(a);
        b = find_ssd(b);
        if (a == b) return;     // already in same set

        if (r[a] < r[b]) {
            swap(a, b);
        }
        p[b] = a;
        if (r[a] == r[b]) {
            r[a]++;
        }
    }
};

int main() {
    int V_ssd;
    cout << "Vertices: ";
    cin >> V_ssd;

    int E_ssd;
    cout << "Edges: ";
    cin >> E_ssd;

    // Edge list: (weight, u, v)
    vector<tuple<int,int,int>> edges_ssd;

    cout << "Enter edges (u v w):\n";
    for (int i = 0; i < E_ssd; i++) {
        int u_ssd, v_ssd, w_ssd;
        cin >> u_ssd >> v_ssd >> w_ssd;
        edges_ssd.emplace_back(w_ssd, u_ssd, v_ssd);
    }

    // Sort edges by weight (non-decreasing)
    sort(edges_ssd.begin(), edges_ssd.end());

    DSU_ssd dsu_ssd(V_ssd);
    int total_ssd = 0;

    cout << "Edges in MST:\n";
    for (auto &t : edges_ssd) {
        int w_ssd, u_ssd, v_ssd;
        tie(w_ssd, u_ssd, v_ssd) = t;

        // If u and v are in different sets, include this edge in MST
        if (dsu_ssd.find_ssd(u_ssd) != dsu_ssd.find_ssd(v_ssd)) {
            dsu_ssd.unite_ssd(u_ssd, v_ssd);
            cout << u_ssd << " - " << v_ssd << " : " << w_ssd << "\n";
            total_ssd += w_ssd;
        }
    }

    cout << "Total MST weight: " << total_ssd << "\n";

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input:**

```text
Vertices: 4
Edges: 5
Enter edges (u v w):
0 1 1
0 2 4
1 2 2
1 3 6
2 3 3
```

**Output (Kruskal):**

```text
Edges in MST:
0 - 1 : 1
1 - 2 : 2
2 - 3 : 3
Total MST weight: 6
```

**Explanation:**

- All edges sorted by weight:  
  - (0–1, 1), (1–2, 2), (2–3, 3), (0–2, 4), (1–3, 6)  
- Kruskal picks:
  - `0 - 1 : 1`  
  - `1 - 2 : 2`  
  - `2 - 3 : 3`  
- These connect all **4 vertices** with no cycles and with **minimum total weight** = `1 + 2 + 3 = 6`.

---

## ✅ **Conclusion**

In this assignment, we:

- Implemented **Kruskal’s Algorithm** using a **sorted edge list**  
- Used **Disjoint Set Union (DSU)** with:
  - **Path compression** in `find_ssd()`  
  - **Union by rank** in `unite_ssd()`  
- Constructed a **Minimum Spanning Tree (MST)** and computed its **total weight**

This program strengthens understanding of:

- **Greedy strategies** in graph algorithms  
- How to prevent **cycles** using **Union-Find**  
- Practical implementation of **MST construction** using an **edge-based approach**.
