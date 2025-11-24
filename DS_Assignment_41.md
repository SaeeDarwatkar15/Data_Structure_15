# 🧩 Assignment 41: Graph Representation Using Adjacency Matrix with BFS and DFS Traversals  

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

## 📘 Problem Statement  
Write a C++ program to:  
1. **Accept a graph from the user** (directed or undirected).  
2. **Represent the graph using an Adjacency Matrix**.  
3. **Perform Breadth-First Search (BFS)** traversal on the graph.  
4. **Perform Depth-First Search (DFS)** traversal on the graph.  

The program should:  
- Allow the user to enter **number of vertices** and **edges**.  
- Accept edges as pairs `(u, v)` where `u` and `v` are vertex numbers.  
- Display the **adjacency matrix** clearly.  
- Ask the user for starting vertices for **BFS** and **DFS** and then display the **traversal order**.  
- Also handle **disconnected graphs** by continuing BFS/DFS for unvisited vertices.

---

## 🧠 Theory  

### 1️⃣ Graph Basics  
A **graph** `G = (V, E)` consists of:  
- `V` → set of **vertices** (nodes)  
- `E` → set of **edges** connecting pairs of vertices  

Graphs can be:  
- **Directed** (edges have direction)  
- **Undirected** (edges are bidirectional)  

### 2️⃣ Adjacency Matrix Representation  
An **adjacency matrix** for a graph with `n` vertices is an `n × n` matrix `A` where:  
- `A[i][j] = 1` if there is an edge from vertex `i` to vertex `j`  
- `A[i][j] = 0` otherwise  

For **undirected** graphs:  
- The matrix is **symmetric**, i.e., `A[i][j] = A[j][i]`.  

Advantages of adjacency matrix:  
- Simple and easy to understand.  
- Fast to check if an edge exists → O(1).  

Disadvantages:  
- Uses O(n²) memory, even if graph is sparse.  

### 3️⃣ Breadth-First Search (BFS)  
**BFS** is a **level-order traversal** algorithm.  
Key ideas:  
- Uses a **queue** data structure (FIFO).  
- Starts from a given **source vertex**.  
- Visits all neighbors of a vertex before going to the next level.  

Steps:  
1. Mark the starting vertex as **visited**, push it into the queue.  
2. While queue is not empty:  
   - Pop front vertex `u`.  
   - Visit all adjacent vertices `v` of `u` (i.e., `adj[u][v] == 1`):  
     - If `v` is not visited → mark visited, push into the queue.  

In this program:  
- BFS is first done from the user’s chosen start vertex.  
- Then, for any vertex still unvisited (disconnected components), BFS is repeated so **all vertices** are covered.  

### 4️⃣ Depth-First Search (DFS)  
**DFS** explores a graph **deeply** before backtracking.  
Key ideas:  
- Implemented using **recursion** or an explicit **stack**.  
- Starts at a vertex, goes as far as possible along one path, then backtracks.  

Steps (recursive):  
1. Mark current vertex `u` as **visited**.  
2. For each neighbor `v` of `u`:  
   - If `v` is not visited → recursively call DFS on `v`.  

In this program:  
- DFS is implemented by a helper `dfsUtil_ssd` (recursive)  
- DFS starts from user’s selected start vertex.  
- Then remaining unvisited vertices are also covered to handle **disconnected graphs**.  

### 5️⃣ Handling Disconnected Graphs  
A graph may have more than one **component**.  
To ensure BFS/DFS visits **all vertices**:  
- After finishing from the chosen start vertex, we loop through all vertices:  
  - If any vertex `i` is still **unvisited**, we start a new BFS/DFS from there.  

---

## 💻 CODE:  

``` cpp
#include <bits/stdc++.h>
using namespace std;

// Print adjacency matrix
void printAdjMatrix_ssd(const vector<vector<int>> &mat_ssd) {
    int n_ssd = mat_ssd.size();
    cout << "\nAdjacency Matrix (" << n_ssd << " x " << n_ssd << "):\n   ";
    for (int j_ssd = 0; j_ssd < n_ssd; ++j_ssd) cout << setw(3) << j_ssd+1;
    cout << "\n";
    for (int i_ssd = 0; i_ssd < n_ssd; ++i_ssd) {
        cout << setw(3) << i_ssd+1;
        for (int j_ssd = 0; j_ssd < n_ssd; ++j_ssd) {
            cout << setw(3) << mat_ssd[i_ssd][j_ssd];
        }
        cout << "\n";
    }
}

// BFS from a given start (0-based). Also continues for disconnected components.
vector<int> bfsTraversal_ssd(const vector<vector<int>> &adj_ssd, int start0_ssd) {
    int n_ssd = adj_ssd.size();
    vector<bool> visited_ssd(n_ssd, false);
    vector<int> order_ssd;
    queue<int> q_ssd;

    // First BFS from the given start vertex
    if (start0_ssd >= 0 && start0_ssd < n_ssd) {
        visited_ssd[start0_ssd] = true;
        q_ssd.push(start0_ssd);
        while (!q_ssd.empty()) {
            int u_ssd = q_ssd.front(); q_ssd.pop();
            order_ssd.push_back(u_ssd);
            for (int v_ssd = 0; v_ssd < n_ssd; ++v_ssd) {
                if (adj_ssd[u_ssd][v_ssd] && !visited_ssd[v_ssd]) {
                    visited_ssd[v_ssd] = true;
                    q_ssd.push(v_ssd);
                }
            }
        }
    }

    // BFS for remaining unvisited vertices (disconnected components)
    for (int s_ssd = 0; s_ssd < n_ssd; ++s_ssd) {
        if (!visited_ssd[s_ssd]) {
            visited_ssd[s_ssd] = true;
            q_ssd.push(s_ssd);
            while (!q_ssd.empty()) {
                int u_ssd = q_ssd.front(); q_ssd.pop();
                order_ssd.push_back(u_ssd);
                for (int v_ssd = 0; v_ssd < n_ssd; ++v_ssd) {
                    if (adj_ssd[u_ssd][v_ssd] && !visited_ssd[v_ssd]) {
                        visited_ssd[v_ssd] = true;
                        q_ssd.push(v_ssd);
                    }
                }
            }
        }
    }
    return order_ssd;
}

// DFS helper (recursive)
void dfsUtil_ssd(const vector<vector<int>> &adj_ssd, int u_ssd, vector<bool> &visited_ssd, vector<int> &order_ssd) {
    visited_ssd[u_ssd] = true;
    order_ssd.push_back(u_ssd);
    int n_ssd = adj_ssd.size();
    for (int v_ssd = 0; v_ssd < n_ssd; ++v_ssd) {
        if (adj_ssd[u_ssd][v_ssd] && !visited_ssd[v_ssd]) {
            dfsUtil_ssd(adj_ssd, v_ssd, visited_ssd, order_ssd);
        }
    }
}

// DFS starting from start0 and covering disconnected components
vector<int> dfsTraversal_ssd(const vector<vector<int>> &adj_ssd, int start0_ssd) {
    int n_ssd = adj_ssd.size();
    vector<bool> visited_ssd(n_ssd, false);
    vector<int> order_ssd;

    if (start0_ssd >= 0 && start0_ssd < n_ssd) {
        dfsUtil_ssd(adj_ssd, start0_ssd, visited_ssd, order_ssd);
    }

    for (int i_ssd = 0; i_ssd < n_ssd; ++i_ssd) {
        if (!visited_ssd[i_ssd]) dfsUtil_ssd(adj_ssd, i_ssd, visited_ssd, order_ssd);
    }
    return order_ssd;
}

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

    int directedChoice_ssd;
    cout << "Is the graph directed? (0 = No, 1 = Yes): ";
    while (!(cin >> directedChoice_ssd) || (directedChoice_ssd != 0 && directedChoice_ssd != 1)) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Enter 0 for undirected or 1 for directed: ";
    }
    bool isDirected_ssd = (directedChoice_ssd == 1);

    vector<vector<int>> adj_ssd(n_ssd, vector<int>(n_ssd, 0));

    int m_ssd;
    cout << "Enter number of edges: ";
    while (!(cin >> m_ssd) || m_ssd < 0) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Please enter a non-negative integer for edges: ";
    }

    cout << "Enter edges (u v) with vertices in range 1.." << n_ssd << ":\n";
    for (int i_ssd = 0; i_ssd < m_ssd; ++i_ssd) {
        int u_ssd, v_ssd;
        cout << "Edge " << i_ssd+1 << ": ";
        while (!(cin >> u_ssd >> v_ssd) || u_ssd < 1 || u_ssd > n_ssd || v_ssd < 1 || v_ssd > n_ssd) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid vertices. Enter again (u v) in 1.." << n_ssd << ": ";
        }
        adj_ssd[u_ssd-1][v_ssd-1] = 1;
        if (!isDirected_ssd) adj_ssd[v_ssd-1][u_ssd-1] = 1;
    }

    printAdjMatrix_ssd(adj_ssd);

    int startBFS_ssd;
    cout << "\nEnter starting vertex for BFS (1.." << n_ssd << ", enter 0 to start from vertex 1): ";
    while (!(cin >> startBFS_ssd) || startBFS_ssd < 0 || startBFS_ssd > n_ssd) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Enter a number between 0 and " << n_ssd << ": ";
    }
    int startBFS0_ssd = (startBFS_ssd == 0 ? 0 : startBFS_ssd-1);

    vector<int> bfsOrder_ssd = bfsTraversal_ssd(adj_ssd, startBFS0_ssd);
    cout << "\nBFS Traversal Order (vertices shown 1-based):\n";
    for (size_t i_ssd = 0; i_ssd < bfsOrder_ssd.size(); ++i_ssd) {
        cout << bfsOrder_ssd[i_ssd] + 1;
        if (i_ssd + 1 < bfsOrder_ssd.size()) cout << " -> ";
    }
    cout << "\n";

    int startDFS_ssd;
    cout << "\nEnter starting vertex for DFS (1.." << n_ssd << ", enter 0 to start from vertex 1): ";
    while (!(cin >> startDFS_ssd) || startDFS_ssd < 0 || startDFS_ssd > n_ssd) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Enter a number between 0 and " << n_ssd << ": ";
    }
    int startDFS0_ssd = (startDFS_ssd == 0 ? 0 : startDFS_ssd-1);

    vector<int> dfsOrder_ssd = dfsTraversal_ssd(adj_ssd, startDFS0_ssd);
    cout << "\nDFS Traversal Order (vertices shown 1-based):\n";
    for (size_t i_ssd = 0; i_ssd < dfsOrder_ssd.size(); ++i_ssd) {
        cout << dfsOrder_ssd[i_ssd] + 1;
        if (i_ssd + 1 < dfsOrder_ssd.size()) cout << " -> ";
    }
    cout << "\n";

    return 0;
}
```
---

## 🧪 Example Execution  

Enter number of vertices: 5  
Is the graph directed? (0 = No, 1 = Yes): 0  
Enter number of edges: 6  
Enter edges (u v) with vertices in range 1..5:  
Edge 1: 1 2  
Edge 2: 1 3  
Edge 3: 2 4  
Edge 4: 3 4  
Edge 5: 4 5  
Edge 6: 2 5  

Adjacency Matrix (5 x 5):  
     1  2  3  4  5  
  1  0  1  1  0  0  
  2  1  0  0  1  1  
  3  1  0  0  1  0  
  4  0  1  1  0  1  
  5  0  1  0  1  0  

Enter starting vertex for BFS (1..5, enter 0 to start from vertex 1): 1  

BFS Traversal Order (vertices shown 1-based):  
1 -> 2 -> 3 -> 4 -> 5  

Enter starting vertex for DFS (1..5, enter 0 to start from vertex 1): 1  

DFS Traversal Order (vertices shown 1-based):  
1 -> 2 -> 4 -> 3 -> 5  

---

## 🧩 Memory / Representation Visualization  

### 🔹 Adjacency Matrix in Memory (for given example)

Vertices: {1, 2, 3, 4, 5}  

Adjacency matrix `adj_ssd[i][j]` (1 = edge, 0 = no edge):  

        j →   1  2  3  4  5  
i ↓  
1           [ 0  1  1  0  0 ]  
2           [ 1  0  0  1  1 ]  
3           [ 1  0  0  1  0 ]  
4           [ 0  1  1  0  1 ]  
5           [ 0  1  0  1  0 ]  

Each row `i` represents outgoing edges from vertex `i`.  
For example:  
- Row 1 → edges from 1 to 2 and 1 to 3  
- Row 2 → edges from 2 to 1, 2 to 4, 2 to 5  

### 🔹 BFS Queue Evolution (Start = 1)

- Start at **1** → visited = {1}, queue = [1]  
- Pop 1 → visit neighbors {2,3} → visited = {1,2,3}, queue = [2,3]  
- Pop 2 → visit neighbors {4,5} → visited = {1,2,3,4,5}, queue = [3,4,5]  
- Pop 3 → neighbors {1,4} but both already visited → queue = [4,5]  
- Pop 4 → neighbors {2,3,5} but all visited → queue = [5]  
- Pop 5 → neighbors {2,4} visited → queue = []  

Order: 1, 2, 3, 4, 5  

### 🔹 DFS Recursion Path (Start = 1)

Call stack pattern:  
- DFS(1) → visit 1  
  - DFS(2) → visit 2  
    - DFS(4) → visit 4  
      - DFS(3) → visit 3  
      - DFS(5) → visit 5  

Order: 1 → 2 → 4 → 3 → 5  

---

## ✅ Conclusion  

This assignment shows how to:  
- **Represent a graph** using an **adjacency matrix**, which is a clean and systematic way to store edge information.  
- Implement **BFS** to explore the graph **level-wise** using a **queue**.  
- Implement **DFS** to explore the graph **depth-wise** using **recursion**.  
- Handle both **directed and undirected graphs** and also **disconnected graphs** by restarting traversals from unvisited vertices.  

It strengthens your understanding of:  
- Graph representation  
- Traversal techniques  
- Use of **queues**, **recursion**, **visited arrays**, and **matrix-based adjacency checks** in real implementations.
