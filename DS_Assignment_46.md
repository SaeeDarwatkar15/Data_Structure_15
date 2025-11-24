# 🧩 **Assignment 46: Kruskal’s Algorithm using Adjacency Matrix**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)** of a **user-defined weighted undirected graph**.  
The graph is represented using an **Adjacency Matrix**, and **Disjoint Set Union (DSU / Union-Find)** is used to efficiently avoid cycles while building the MST.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted, undirected graph** from the user in the form of an **Adjacency Matrix**  
- Use **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)**  
- Display:
  - All **edges** selected in the MST  
  - The **total weight** of the MST  

Assume:
- `0` or `-1` in the adjacency matrix means **no edge** between those vertices.  
- Positive values represent the **weight** of the edge.

---

## 📘 **Theory**

### 🌳 1. **Minimum Spanning Tree (MST)**

Given a **connected, undirected, weighted graph**:

- A **Spanning Tree**:
  - Includes **all vertices**
  - Has exactly **(V − 1)** edges
  - Has **no cycles**

- A **Minimum Spanning Tree (MST)** is a spanning tree with the **minimum possible total edge weight**.

---

### 🧮 2. **Kruskal’s Algorithm – Greedy Approach**

Kruskal’s Algorithm is a **greedy algorithm** that builds the MST by:

1. Considering all **edges** of the graph.  
2. Sorting these edges in **non-decreasing order** of their **weights**.  
3. Starting from the **smallest edge**:
   - Add the edge to the MST **only if** it does **not form a cycle** with already included edges.  
4. Repeat until MST has **(V − 1)** edges.

To check whether adding an edge forms a **cycle**, we use **Disjoint Set Union (DSU / Union-Find)**.

---

### 🧩 3. **Disjoint Set Union (DSU / Union-Find)**

**DSU** is a structure that keeps track of a partition of elements into **disjoint sets**.

Operations:

- `find_ssd(x)`  
  - Finds the **representative (root)** of the set containing `x`.  
  - Uses **path compression** to speed up future queries.

- `unite_ssd(a, b)`  
  - Merges the sets containing `a` and `b`.  
  - Uses **union by rank** to keep the tree shallow.

In Kruskal’s Algorithm:

- If `find_ssd(u) != find_ssd(v)` → `u` and `v` are in **different components** → adding edge `(u, v)` will **not** form a cycle → **include** it in MST.  
- Else → edge is **skipped**.

---

### 📂 4. **Adjacency Matrix Representation**

The graph is represented as:

```cpp
vector<vector<int>> W_ssd(V_ssd, vector<int>(V_ssd));
```

- `W_ssd[i][j]` contains the **weight** of the edge between vertex `i` and vertex `j`.  
- If `W_ssd[i][j]` is:
  - `0` or `-1` → **no edge**  
  - `> 0` → edge present with that **weight**  

For an undirected graph, the matrix is **symmetric**:
- `W_ssd[i][j] == W_ssd[j][i]`

To avoid processing the same edge twice, we only consider pairs where `j > i`.

---

### ⏱️ 5. **Time Complexity**

Let:

- `V` = number of vertices  
- `E` = number of edges  

Steps:

1. Extracting edges from adjacency matrix → `O(V²)`  
2. Sorting edges → `O(E log E)`  
3. DSU operations while building MST → almost `O(E)` (amortized)

For dense graphs (Adjacency Matrix), `V²` dominates.

---

### 🌐 6. **Applications**

Kruskal’s Algorithm and MST are widely used in:

- **Network design** (laying cables, telephone lines, LAN, WAN)  
- **Transportation networks** (roads, rails)  
- **Clustering algorithms** in Machine Learning  
- Any scenario minimizing **total connection cost**

---

## 💻 **CODE**

```cpp

#include <bits/stdc++.h>
using namespace std;

// Disjoint Set Union (Union-Find) with path compression and union by rank
struct DSU_ssd {
    vector<int> parent_ssd, rank_ssd;

    DSU_ssd(int n_ssd) {
        parent_ssd.resize(n_ssd);
        rank_ssd.assign(n_ssd, 0);
        for (int i_ssd = 0; i_ssd < n_ssd; i_ssd++) {
            parent_ssd[i_ssd] = i_ssd;
        }
    }

    int find_ssd(int x_ssd) {
        if (parent_ssd[x_ssd] == x_ssd)
            return x_ssd;
        // Path compression
        return parent_ssd[x_ssd] = find_ssd(parent_ssd[x_ssd]);
    }

    void unite_ssd(int a_ssd, int b_ssd) {
        a_ssd = find_ssd(a_ssd);
        b_ssd = find_ssd(b_ssd);
        if (a_ssd == b_ssd) return;

        // Union by rank
        if (rank_ssd[a_ssd] < rank_ssd[b_ssd]) {
            swap(a_ssd, b_ssd);
        }
        parent_ssd[b_ssd] = a_ssd;
        if (rank_ssd[a_ssd] == rank_ssd[b_ssd]) {
            rank_ssd[a_ssd]++;
        }
    }
};

int main() {
    int V_ssd;
    cout << "Enter number of vertices: ";
    cin >> V_ssd;

    // Adjacency matrix
    vector<vector<int>> W_ssd(V_ssd, vector<int>(V_ssd));
    cout << "Enter adjacency matrix (use 0 or -1 for no edge):\n";
    for (int i_ssd = 0; i_ssd < V_ssd; i_ssd++) {
        for (int j_ssd = 0; j_ssd < V_ssd; j_ssd++) {
            cin >> W_ssd[i_ssd][j_ssd];
        }
    }

    // Build edge list: (weight, u, v)
    vector<tuple<int,int,int>> edges_ssd;
    for (int i_ssd = 0; i_ssd < V_ssd; i_ssd++) {
        for (int j_ssd = i_ssd + 1; j_ssd < V_ssd; j_ssd++) {
            int w_ssd = W_ssd[i_ssd][j_ssd];
            // Consider only positive weights as valid edges
            if (w_ssd > 0) {
                edges_ssd.emplace_back(w_ssd, i_ssd, j_ssd);
            }
        }
    }

    // Sort edges by weight (ascending)
    sort(edges_ssd.begin(), edges_ssd.end());

    DSU_ssd dsu_ssd(V_ssd);
    int count_ssd = 0;
    int total_ssd = 0;

    cout << "Edges in MST (u - v : w):\n";
    for (auto &edge_ssd : edges_ssd) {
        int w_ssd, u_ssd, v_ssd;
        tie(w_ssd, u_ssd, v_ssd) = edge_ssd;

        // If u and v are in different sets, this edge can be added
        if (dsu_ssd.find_ssd(u_ssd) != dsu_ssd.find_ssd(v_ssd)) {
            dsu_ssd.unite_ssd(u_ssd, v_ssd);
            cout << u_ssd << " - " << v_ssd << " : " << w_ssd << "\n";
            total_ssd += w_ssd;
            count_ssd++;

            // If we already have V-1 edges, MST is complete
            if (count_ssd == V_ssd - 1) {
                break;
            }
        }
    }

    cout << "Total MST weight: " << total_ssd << "\n";

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input**  
(Adjacency matrix for 4 vertices; `0` means no edge):

```text
Enter number of vertices: 4
Enter adjacency matrix (use 0 or -1 for no edge):
0 1 4 0
1 0 2 6
4 2 0 3
0 6 3 0
```

**Output**

```text
Edges in MST (u - v : w):
0 - 1 : 1
1 - 2 : 2
2 - 3 : 3
Total MST weight: 6
```

Explanation:

- Selected edges:
  - `(0, 1)` with weight `1`
  - `(1, 2)` with weight `2`
  - `(2, 3)` with weight `3`
- Total MST weight = `1 + 2 + 3 = 6`  

All **4 vertices** are connected with **minimum possible total cost**.

---

## ✅ **Conclusion**

In this assignment, we:

- Represented a **weighted undirected graph** using an **Adjacency Matrix**  
- Implemented **Kruskal’s Algorithm** to find the **Minimum Spanning Tree (MST)**  
- Used **Disjoint Set Union (DSU)** with **path compression** and **union by rank** to:
  - Efficiently check whether adding an edge forms a **cycle**  
  - Merge components as edges are added  

This program strengthens understanding of:

- **Greedy algorithms** on graphs  
- **MST construction** using edge-based approach  
- Practical use of **Adjacency Matrix** and **Union-Find data structure** in solving graph problems.
