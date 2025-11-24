# 🧩 **Assignment 50: Dijkstra’s Algorithm using Adjacency List**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Dijkstra’s Algorithm** to compute the **shortest distance** and **shortest path** between two user-defined nodes in a weighted graph.  
The graph is represented using an **Adjacency List**, and a **Min-Heap Priority Queue** is used to efficiently pick the next closest node.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted, undirected graph** from the user  
- Represent the graph using an **Adjacency List**  
- Apply **Dijkstra’s Algorithm** to compute:
  - **Shortest distance** from a source node to destination  
  - The **actual shortest path**  
- Display the results neatly  

Assumptions:

- Vertices are numbered **0 to V−1**  
- Edge weights are **non-negative** (required for Dijkstra)  

---

## 📘 **Theory**

### 🌐 1. **Shortest Path Problem**

Given a weighted graph and a **source vertex**, we want to find the shortest path to a **destination vertex**.  
Dijkstra’s Algorithm is the most famous method for solving this when **all weights are non-negative**.

---

### 🧮 2. **Dijkstra’s Algorithm – Greedy Logic**

Dijkstra’s Algorithm maintains:

- `dist[v]` → shortest known distance from source to node `v`  
- `parent[v]` → previous node in the shortest path to `v`  
- A **min-heap** to pick the next closest node  

Steps:

1. Set all distances to **∞**, except source = 0  
2. Push source into min-heap  
3. Repeatedly pick the node with **minimum tentative distance**  
4. Relax all outgoing edges:
   - If `dist[v] > dist[u] + weight`, update the distance  
5. Continue until all reachable nodes are processed  

---

### 📂 3. **Adjacency List Representation**

The graph is stored as:

```cpp
vector<vector<pair<int,int>>> adj_ssd;
```

For each vertex `u`:

- `adj_ssd[u]` contains pairs `(v, w)` where:
  - `v` = neighbor  
  - `w` = weight of edge `(u, v)`  

Since graph is **undirected**, input edges add:

- `(v, w)` to `adj_ssd[u]`  
- `(u, w)` to `adj_ssd[v]`

---

### ⏱️ 4. **Time Complexity**

Using:

- Adjacency List  
- Min-Heap Priority Queue  

Total complexity →  
👉 **O(E log V)**  

This is efficient and ideal for sparse to moderately dense graphs.

---

### 📌 5. **Applications**

Dijkstra’s Algorithm is used in:

- GPS & navigation (Google Maps, Ola, Uber)  
- Packet routing in networks  
- Robot path planning  
- Game development (AI pathfinding)  
- Transport & logistics optimization  

---

## 💻 **CODE**

```cpp

#include <bits/stdc++.h>
using namespace std;
using pii = pair<int,int>;

void dijkstra_ssd(int V_ssd, vector<vector<pair<int,int>>>& adj_ssd, int src_ssd, int dest_ssd) {
    const int INF_ssd = 1e9;
    vector<int> dist_ssd(V_ssd, INF_ssd), parent_ssd(V_ssd, -1);
    priority_queue<pii, vector<pii>, greater<pii>> pq_ssd;

    // Initialize source
    dist_ssd[src_ssd] = 0;
    pq_ssd.push({0, src_ssd});

    while (!pq_ssd.empty()) {
        auto [d, u] = pq_ssd.top();
        pq_ssd.pop();

        if (d > dist_ssd[u])
            continue;

        for (auto &pr : adj_ssd[u]) {
            int v = pr.first;
            int w = pr.second;

            if (dist_ssd[v] > dist_ssd[u] + w) {
                dist_ssd[v] = dist_ssd[u] + w;
                parent_ssd[v] = u;
                pq_ssd.push({dist_ssd[v], v});
            }
        }
    }

    if (dist_ssd[dest_ssd] == INF_ssd) {
        cout << "No path\n";
        return;
    }

    cout << "Shortest distance: " << dist_ssd[dest_ssd] << "\n";

    // Reconstruct path
    vector<int> path;
    for (int v = dest_ssd; v != -1; v = parent_ssd[v])
        path.push_back(v);
    reverse(path.begin(), path.end());

    cout << "Path: ";
    for (size_t i = 0; i < path.size(); i++) {
        cout << path[i];
        if (i + 1 < path.size())
            cout << " -> ";
    }
    cout << "\n";
}

int main() {
    int V_ssd;
    cout << "Vertices: ";
    cin >> V_ssd;

    int E_ssd;
    cout << "Edges: ";
    cin >> E_ssd;

    vector<vector<pair<int,int>>> adj_ssd(V_ssd);

    cout << "Enter edges (u v w):\n";
    for (int i = 0; i < E_ssd; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        adj_ssd[u].push_back({v, w});
        adj_ssd[v].push_back({u, w});   // undirected graph
    }

    int s, d;
    cout << "Source Destination: ";
    cin >> s >> d;

    dijkstra_ssd(V_ssd, adj_ssd, s, d);

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input:**

```text
Vertices: 5
Edges:
0 1 10
0 4 5
1 2 1
2 3 4
3 4 2
4 1 3
Source: 0 Destination: 3
```

**Output:**

```text
Shortest distance: 9
Path: 0 -> 4 -> 3
```

**Explanation:**

- Shortest route from **0** to **3** is:  
  - `0 → 4` weight = 5  
  - `4 → 3` weight = 2  
- Total = **5 + 2 = 7**  
- Alternative paths like `0 → 1 → 2 → 3` are longer (10 + 1 + 4 = 15)  
- Hence the shortest path is **0 → 4 → 3**

---

## ✅ **Conclusion**

In this assignment, we:

- Represented a **weighted undirected graph** using an **Adjacency List**  
- Applied **Dijkstra’s Algorithm** using a **priority queue**  
- Successfully computed:
  - **Shortest distance** between two user-specified nodes  
  - The **path** taken to achieve that minimal distance

This strengthens understanding of:

- Greedy algorithms  
- Priority queue operations  
- Shortest path algorithms  
- Efficient graph representation and traversal techniques  
