# 🧩 **Assignment 47: Dijkstra’s Algorithm using Adjacency Matrix**

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  
**Notes:**  
This program implements **Dijkstra’s Algorithm** to find the **shortest distance between two nodes** of a **user-defined weighted graph**.  
The graph is represented using an **Adjacency Matrix**, and the algorithm uses a **greedy approach** to iteratively choose the next closest unvisited vertex.

---

## 📝 **Problem Statement**

Write a **C++ program** to:

- Accept a **weighted graph** from the user in the form of an **Adjacency Matrix**  
- Use **Dijkstra’s Algorithm** to find the **shortest distance** between a **source node** and a **destination node**  
- Display:
  - The **shortest distance value**  
  - The **actual shortest path** (sequence of vertices from source to destination)

Assumptions:

- Graph may be **undirected** or **directed**, but represented via adjacency matrix.  
- **Edge weights are non-negative**, as required by standard Dijkstra’s Algorithm.  
- `0` or `-1` in the matrix indicates **no edge** between two vertices.

---

## 📘 **Theory**

### 🌐 1. **Shortest Path Problem**

Given a **weighted graph** and a **source vertex**, the **single-source shortest path problem** is to find the minimum cost path from the source to all other vertices.

In this assignment, we are particularly interested in the shortest path between a **given source** and **given destination**.

---

### 🧮 2. **Dijkstra’s Algorithm – Concept**

Dijkstra’s Algorithm is a **greedy algorithm** that solves the **single-source shortest path problem** for graphs with **non-negative edge weights**.

**Key idea:**

1. Maintain an array `dist[]` where `dist[v]` is the **current known shortest distance** from the source to vertex `v`.  
2. Initialize all distances to **infinity**, except the source which is `0`.  
3. Repeatedly:
   - Pick the **unvisited vertex** with the **smallest distance** value from `dist[]`.  
   - Mark it as **visited**.  
   - Relax all edges from this vertex:
     - If going through this vertex gives a **shorter path** to a neighbor, update that neighbor’s distance.  

The algorithm stops when all vertices are processed or no further reachable vertices exist.

---

### 📂 3. **Adjacency Matrix Representation**

The graph is stored as:

```cpp
vector<vector<int>> W_ssd(V_ssd, vector<int>(V_ssd));
```

- `W_ssd[i][j]` represents the **weight** of the edge from vertex `i` to vertex `j`.  
- Values:
  - `0` or `-1` → **no edge**  
  - Positive integer → **weight** of that edge

Using an adjacency matrix makes it easy to:

- Access the weight between any pair of vertices in **O(1)** time  
- Traverse all neighbors of a vertex by scanning its **row** in `O(V)` time

---

### 📊 4. **Distance, Visited, and Parent Arrays**

- `dist_ssd[v]`  
  - Current shortest distance from **source** to vertex `v`  
  - Initialized to a large value `INF_ssd` for all except source

- `visited_ssd[v]`  
  - Marks whether vertex `v` is **already processed** (final distance fixed)

- `parent_ssd[v]`  
  - Stores the **previous vertex** on the shortest path from source to `v`  
  - Used later to reconstruct and print the **actual path**

---

### ⏱️ 5. **Time Complexity**

Let:

- `V` = number of vertices  

In this implementation (using an adjacency matrix and a simple linear search for the minimum):

- Outer loop runs `V` times  
- Inner selection of minimum distance vertex runs `O(V)`  
- Relaxing edges for all vertices runs `O(V)` per iteration  

So overall time complexity is:  
👉 **O(V²)**

This is acceptable for **small to medium graphs** and is natural with an adjacency matrix representation.

---

### 📌 6. **Applications**

Dijkstra’s Algorithm is widely used in:

- **Navigation and routing systems** (GPS, Google Maps)  
- **Network routing protocols** (finding least-cost path)  
- **Game development** for AI pathfinding  
- Any system needing **shortest path in weighted graphs with non-negative weights**

---

## 💻 **CODE**

```cpp

#include <bits/stdc++.h>
using namespace std;
const int INF_ssd = 1e9;

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

    int src_ssd, dest_ssd;
    cout << "Enter source and destination: ";
    cin >> src_ssd >> dest_ssd;

    vector<int> dist_ssd(V_ssd, INF_ssd);
    vector<int> visited_ssd(V_ssd, 0);
    vector<int> parent_ssd(V_ssd, -1);

    // Distance to source is 0
    dist_ssd[src_ssd] = 0;

    // Dijkstra's algorithm (O(V^2) using adjacency matrix)
    for (int i_ssd = 0; i_ssd < V_ssd; i_ssd++) {
        int u_ssd = -1;
        int best_ssd = INF_ssd;

        // Pick the unvisited vertex with the smallest distance
        for (int v_ssd = 0; v_ssd < V_ssd; v_ssd++) {
            if (!visited_ssd[v_ssd] && dist_ssd[v_ssd] < best_ssd) {
                best_ssd = dist_ssd[v_ssd];
                u_ssd = v_ssd;
            }
        }

        // No reachable unvisited vertex remains
        if (u_ssd == -1) break;

        visited_ssd[u_ssd] = 1;

        // Relax edges from u_ssd to all v_ssd
        for (int v_ssd = 0; v_ssd < V_ssd; v_ssd++) {
            int w_ssd = W_ssd[u_ssd][v_ssd];
            // w_ssd > 0 means there is an edge (ignore 0 or -1 as no edge)
            if (w_ssd > 0 && !visited_ssd[v_ssd] &&
                dist_ssd[v_ssd] > dist_ssd[u_ssd] + w_ssd) {

                dist_ssd[v_ssd] = dist_ssd[u_ssd] + w_ssd;
                parent_ssd[v_ssd] = u_ssd;
            }
        }
    }

    // If destination is unreachable
    if (dist_ssd[dest_ssd] == INF_ssd) {
        cout << "No path\n";
        return 0;
    }

    // Print shortest distance
    cout << "Shortest distance: " << dist_ssd[dest_ssd] << "\n";

    // Reconstruct path from dest_ssd back to src_ssd
    vector<int> path_ssd;
    for (int v_ssd = dest_ssd; v_ssd != -1; v_ssd = parent_ssd[v_ssd]) {
        path_ssd.push_back(v_ssd);
    }
    reverse(path_ssd.begin(), path_ssd.end());

    // Print path
    cout << "Path: ";
    for (size_t i_ssd = 0; i_ssd < path_ssd.size(); ++i_ssd) {
        cout << path_ssd[i_ssd];
        if (i_ssd + 1 < path_ssd.size()) {
            cout << " -> ";
        }
    }
    cout << "\n";

    return 0;
}
```

---

## 🧮 **Example Execution**

**Input (4 vertices):**

```text
Enter number of vertices: 4
Enter adjacency matrix (use 0 or -1 for no edge):
0 5 0 0
5 0 2 6
0 2 0 3
0 6 3 0
Enter source and destination: 0 3
```

**Output:**

```text
Shortest distance: 10
Path: 0 -> 1 -> 2 -> 3
```

**Explanation:**

- Path chosen by Dijkstra’s Algorithm:
  - `0 → 1` with weight `5`
  - `1 → 2` with weight `2`
  - `2 → 3` with weight `3`
- Total distance = `5 + 2 + 3 = 10`  
- This is the **minimum possible distance** between vertex `0` and vertex `3` in the given graph.

---

## ✅ **Conclusion**

In this assignment, we implemented **Dijkstra’s Algorithm** using:

- An **Adjacency Matrix** to represent the graph  
- Arrays for:
  - **Distances (`dist_ssd`)**  
  - **Visited vertices (`visited_ssd`)**  
  - **Parents (`parent_ssd`)** to reconstruct the shortest path  

The program:

- Accepts a **user-defined graph**  
- Computes the **shortest distance** from a **source** to a **destination**  
- Prints the **actual shortest path**  

This strengthens understanding of:

- **Single-source shortest path algorithms**  
- How **Dijkstra’s Algorithm** works with an adjacency matrix in **O(V²)** time  
- The importance of **non-negative weights** for correct shortest path computation.
