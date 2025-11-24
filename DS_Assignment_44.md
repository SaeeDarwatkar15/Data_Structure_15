# 🧩 **Assignment 44: Dijkstra’s Algorithm using Adjacency List**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Dijkstra’s Algorithm** to find the **shortest distance** between **two nodes** of a **user-defined weighted graph**.  
The graph is represented using an **Adjacency List**, and a **Min-Heap (Priority Queue)** is used to efficiently pick the next closest vertex.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted graph** from the user  
- Represent the graph using an **Adjacency List**  
- Use **Dijkstra’s Algorithm** to find the **shortest distance** between a **source node** and a **destination node**  
- Display:
  - The **shortest distance**  
  - The **actual path** from source to destination  

Assume that all **edge weights are non-negative** (a requirement for standard Dijkstra’s Algorithm).

---

## 📘 **Theory**

### 🌐 1. **Shortest Path Problem**

Given a **weighted graph** where edges have **non-negative weights**, the **single-source shortest path problem** is to find the minimum cost path from a **source vertex** to all other vertices.

In this assignment, we focus on:

- Finding the **shortest path from a single source** to a **single destination**.

---

### 🧮 2. **Dijkstra’s Algorithm – Basic Idea**

Dijkstra’s Algorithm is a classic **greedy algorithm** that solves the **single-source shortest path** problem for graphs with **non-negative edge weights**.

**Main Idea:**

1. Start from the **source vertex**, with distance = 0.  
2. At each step, choose the **unvisited vertex with the smallest known distance**.  
3. Relax all **outgoing edges** of this vertex:
   - If going through this vertex gives a **shorter distance** to a neighbor, update that neighbor’s distance.  
4. Repeat until all reachable vertices are processed or the destination is confirmed.

---

### 📂 3. **Adjacency List Representation**

We use:

```cpp
vector<vector<pair<int,int>>> adj_ssd;
```

For each vertex `u_ssd`:

- `adj_ssd[u_ssd]` is a vector of pairs `(v_ssd, w_ssd)` where:  
  - `v_ssd` = neighboring vertex  
  - `w_ssd` = weight of the edge `(u_ssd, v_ssd)`

Advantages:

- Efficient for **sparse graphs**  
- Easy to traverse neighbors of a vertex  

---

### ⏱️ 4. **Priority Queue (Min-Heap)**

We use:

```cpp
priority_queue<pii, vector<pii>, greater<pii>> pq_ssd;
```

where `pii = pair<int,int>` represents:

- `first`  → current **distance**  
- `second` → **vertex index**

The priority queue always gives us the vertex with the **smallest distance so far**, making Dijkstra efficient.

---

### 📊 5. **Distance and Parent Arrays**

- `dist_ssd[v]`  
  - Stores the **current shortest distance** from **source** to vertex `v`.  
  - Initialized with a large value `INF_ssd` for all vertices except the source.

- `parent_ssd[v]`  
  - Stores the **previous vertex** in the shortest path from source to `v`.  
  - Used to reconstruct and print the **shortest path**.

---

### ⏱️ 6. **Time Complexity**

Let:

- `V` = number of vertices  
- `E` = number of edges  

Using:

- **Adjacency List**  
- **Min-Heap (priority queue)**  

Time Complexity:  
👉 **O(E log V)**

---

### 📌 7. **Applications**

Dijkstra’s Algorithm is widely used in:

- **Routing & Navigation systems** (Google Maps, GPS)  
- **Network routing protocols** (OSPF, IS-IS)  
- **Game development** (AI pathfinding)  
- Any application where **shortest path in weighted graphs** is needed.

---

## 🔹 **Algorithm Steps (Dijkstra from Source to Destination)**

1. Input number of vertices `V_ssd` and edges `E_ssd`.  
2. Create an **Adjacency List** of size `V_ssd`.  
3. Input each edge `(u_ssd, v_ssd, w_ssd)` and store:
   - `adj_ssd[u_ssd].push_back({v_ssd, w_ssd});`
   - `adj_ssd[v_ssd].push_back({u_ssd, w_ssd});` (since graph is undirected here).  
4. Input **source** and **destination** vertices.  
5. Initialize:
   - `dist_ssd[]` = `INF_ssd` for all vertices  
   - `dist_ssd[src_ssd] = 0`  
   - `parent_ssd[] = -1`  
6. Push `(0, src_ssd)` into the **priority queue**.  
7. While the queue is not empty:
   - Pop vertex `u_ssd` with the **smallest distance d_ssd**.  
   - If this distance is outdated (`d_ssd > dist_ssd[u_ssd]`), skip.  
   - For each neighbor `(v_ssd, w_ssd)` of `u_ssd`:
     - If `dist_ssd[v_ssd] > dist_ssd[u_ssd] + w_ssd`:
       - Update `dist_ssd[v_ssd]`  
       - Update `parent_ssd[v_ssd]`  
       - Push new distance into the queue.  
8. After loop:
   - If `dist_ssd[dest_ssd]` is still `INF_ssd` → **no path** exists.  
   - Else:
     - Print **shortest distance**  
     - Reconstruct path using `parent_ssd[]` from destination back to source  
     - Print full path.

---

## 💻 **CODE**

```cpp
#include <bits/stdc++.h>
using namespace std;
using pii = pair<int,int>; // (dist, vertex)

// Dijkstra's algorithm to find shortest path from src_ssd to dest_ssd
void dijkstra_ssd(int V_ssd, vector<vector<pair<int,int>>>& adj_ssd, int src_ssd, int dest_ssd) {
    const int INF_ssd = 1e9;
    vector<int> dist_ssd(V_ssd, INF_ssd);
    vector<int> parent_ssd(V_ssd, -1);
    priority_queue<pii, vector<pii>, greater<pii>> pq_ssd;

    // Initialize source
    dist_ssd[src_ssd] = 0;
    pq_ssd.push({0, src_ssd});

    while (!pq_ssd.empty()) {
        auto [d_ssd, u_ssd] = pq_ssd.top();
        pq_ssd.pop();

        // If this distance is not the latest, skip
        if (d_ssd > dist_ssd[u_ssd]) 
            continue;

        // Relax edges from u_ssd
        for (auto &edge_ssd : adj_ssd[u_ssd]) {
            int v_ssd = edge_ssd.first;
            int w_ssd = edge_ssd.second;
            if (dist_ssd[v_ssd] > dist_ssd[u_ssd] + w_ssd) {
                dist_ssd[v_ssd] = dist_ssd[u_ssd] + w_ssd;
                parent_ssd[v_ssd] = u_ssd;
                pq_ssd.push({dist_ssd[v_ssd], v_ssd});
            }
        }
    }

    // If destination is unreachable
    if (dist_ssd[dest_ssd] == INF_ssd) {
        cout << "No path from " << src_ssd << " to " << dest_ssd << "\n";
        return;
    }

    // Print shortest distance
    cout << "Shortest distance: " << dist_ssd[dest_ssd] << "\n";

    // Reconstruct and print path
    vector<int> path_ssd;
    for (int v_ssd = dest_ssd; v_ssd != -1; v_ssd = parent_ssd[v_ssd]) {
        path_ssd.push_back(v_ssd);
    }
    reverse(path_ssd.begin(), path_ssd.end());

    cout << "Path: ";
    for (int x_ssd : path_ssd) {
        cout << x_ssd;
        if (x_ssd != dest_ssd)
            cout << " -> ";
    }
    cout << "\n";
}

int main() {
    int V_ssd;
    cout << "Enter number of vertices: ";
    cin >> V_ssd;

    int E_ssd;
    cout << "Enter number of edges: ";
    cin >> E_ssd;

    vector<vector<pair<int,int>>> adj_ssd(V_ssd);
    cout << "Enter edges (u v w):\n";
    for (int i_ssd = 0; i_ssd < E_ssd; ++i_ssd) {
        int u_ssd, v_ssd, w_ssd;
        cin >> u_ssd >> v_ssd >> w_ssd;
        // assuming 0-based vertices
        adj_ssd[u_ssd].push_back({v_ssd, w_ssd});
        adj_ssd[v_ssd].push_back({u_ssd, w_ssd}); // if graph is undirected
    }

    int src_ssd, dest_ssd;
    cout << "Enter source and destination: ";
    cin >> src_ssd >> dest_ssd;

    dijkstra_ssd(V_ssd, adj_ssd, src_ssd, dest_ssd);

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input / Output (Sample Run)**

```text
Enter number of vertices: 5
Enter number of edges: 6
Enter edges (u v w):
0 1 10
0 4 5
1 2 1
2 3 4
3 4 2
4 1 3
Enter source and destination: 0 3
Shortest distance: 9
Path: 0 -> 4 -> 3
```

Explanation:

- The shortest path from vertex **0** to vertex **3** is:  
  **0 → 4 → 3**  
- Path cost:
  - Edge (0, 4) = 5  
  - Edge (4, 3) = 4  
  - Total = **5 + 4 = 9**

---

## ✅ **Conclusion**

In this assignment, we implemented **Dijkstra’s Algorithm** using:

- **Adjacency List** for graph representation  
- **Priority Queue (Min-Heap)** to always pick the vertex with the **minimum tentative distance**  
- **Distance and Parent arrays** to compute and reconstruct the shortest path  

This program:

- Computes the **shortest distance** between a **given source and destination**  
- Prints the **actual shortest path**  
- Reinforces key concepts of **greedy algorithms**, **graph representation**, and **shortest path computation** in weighted graphs with **non-negative edge weights**.
