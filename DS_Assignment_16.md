# 🧩 Assignment 16: Set Implementation Using Generalized Linked List (GLL)

**Author:** Saee Darwatkar  
**Institute:** Vishwakarma Institute of Technology  
**Department:** Computer Engineering  
**Subject:** Data Structures  

**Aim:**  
To implement a **Set** using a **Generalized Linked List (GLL)** in C++ and store a nested set structure like:  

\[
S = \{ p, q, \{ r, s, t, \{\}, \{u, v\}, w, x, \{y, z\}, a1, b1 \} \}
\]

The program should display this structure in **proper set notation format**.

---

## 📝 Problem Statement

Write a C++ program to:

- Represent a **set with nested subsets** using a **Generalized Linked List (GLL)**.  
- Handle:
  - **Atoms** → simple elements like `p`, `q`, `r`, `a1`, `b1`  
  - **Sublists** → nested sets like `{}`, `{u, v}`, `{y, z}`, and the big inner set itself  
- Display the full structure in **correct set notation**, including nested `{}`.

Given example set:

\[
S = \{ p, q, \{ r, s, t, \{\}, \{u, v\}, w, x, \{y, z\}, a1, b1 \} \}
\]

---

## 📘 Theory

### 🔹 Generalized Linked List (GLL)

A **Generalized Linked List** is used to represent **hierarchical / nested structures** such as sets containing:

- Individual elements (atoms)
- Other sets (sublists)

Each node has:

- `flag_ssd`  
  - `0` → **Atom node** (simple data)  
  - `1` → **Sublist node** (points to another list)
- `data_ssd` → string value (used only for atom nodes)
- `next_ssd` → pointer to next element at the **same level**
- `sublist_ssd` → pointer to **nested list** (used when `flag_ssd = 1`)

This allows representation of sets like:

```text
{ p, q, { r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1 } }
```

with arbitrary nesting depth.

---

### 🔹 Representation of the Given Set S

We want to represent:

\[
S = \{ p, q, \{ r, s, t, \{\}, \{u, v\}, w, x, \{y, z\}, a1, b1 \} \}
\]

Breakdown:

- **Level 1 (Main Set S)**  
  Elements:
  - `p` (atom)
  - `q` (atom)
  - A **sublist**:
    \{ r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1 \}

- **Level 2 (Inner Subset)**  
  Elements:
  - `r`, `s`, `t` (atoms)
  - `{}` → empty set (sublist with `sublist_ssd = NULL`)
  - `{u, v}` → sublist containing atoms `u`, `v`
  - `w`, `x` (atoms)
  - `{y, z}` → sublist containing atoms `y`, `z`
  - `a1`, `b1` (atoms)

- **Level 3**
  - `{}` → sublist pointing to `NULL`
  - `{u, v}` → sublist → `u -> v`
  - `{y, z}` → sublist → `y -> z`

---

### 🔹 Printing the GLL

To print in correct set notation:

- Use recursion:
  - Print `{`
  - For each node:
    - If `flag_ssd == 0` → print `data_ssd` (atom)
    - If `flag_ssd == 1` → **recursively call** `printGLL_ssd` on `sublist_ssd`
  - Separate elements with `, `
  - End with `}`

Example output format:

```text
{p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1}}
```

---

## 🧠 Algorithm (High-Level)

### 1️⃣ Node Structure

Define `struct gll_ssd` with:

- `int flag_ssd`
- `string data_ssd`
- `gll_ssd* next_ssd`
- `gll_ssd* sublist_ssd`

### 2️⃣ Node Creation

`getNode_ssd(flag, data)`:

1. Allocate new node.
2. Set:
   - `flag_ssd = flag`
   - `data_ssd = data`
   - `next_ssd = nullptr`
   - `sublist_ssd = nullptr`
3. Return pointer.

---

### 3️⃣ Build the Given Set S

In `buildSet_ssd()`:

- Create **level 1 nodes**:
  - `p_ssd` (atom, `"p"`)
  - `q_ssd` (atom, `"q"`)
  - `sub_ssd` (sublist node, flag = 1)

- Link: `p_ssd -> q_ssd -> sub_ssd`

- For **level 2** (inner set):
  - Atoms: `r`, `s`, `t`, `w`, `x`, `a1`, `b1`
  - Sublists:
    - `empty_ssd` for `{}`
    - `uv_ssd` for `{u, v}`
    - `yz_ssd` for `{y, z}`

- Link level-2 nodes in order:
  - `r -> s -> t -> empty -> uv -> w -> x -> yz -> a1 -> b1`

- Attach level-2 head to:
  - `sub_ssd->sublist_ssd = r_ssd`

- For **level 3**:
  - `empty_ssd->sublist_ssd = nullptr`
  - `{u, v}`:
    - `uv_ssd->sublist_ssd = u`
    - `u->next_ssd = v`
  - `{y, z}`:
    - `yz_ssd->sublist_ssd = y`
    - `y->next_ssd = z`

Return `p_ssd` as head of the main set.

---

### 4️⃣ Print the GLL

`printGLL_ssd(head)`:

1. Print `{`.
2. Traverse from `head`:
   - If `flag_ssd == 0` → print `data_ssd`.
   - If `flag_ssd == 1` → recursively call `printGLL_ssd(sublist_ssd)`.
   - If `next_ssd != nullptr` → print `, `.
3. End with `}`.

---

### 5️⃣ Destroy the GLL

`destroyGLL_ssd(head)`:

1. Traverse nodes at current level.
2. If `flag_ssd == 1`, call `destroyGLL_ssd` for `sublist_ssd`.
3. Delete each node to free memory.

---

## 💻 C++ Program

```cpp
#include <iostream>
#include <string>

using namespace std;

// Node structure for GLL
struct gll_ssd {
    int flag_ssd;             // 0 = atom, 1 = sublist
    string data_ssd;          // atom data (string)
    gll_ssd* next_ssd;        // next node at same level
    gll_ssd* sublist_ssd;     // pointer to sublist if flag = 1
};

// Create a new node
gll_ssd* getNode_ssd(int flag_ssd, const string& data_ssd) {
    gll_ssd* newnode_ssd = new gll_ssd;
    if (!newnode_ssd) {
        cout << "Memory allocation failed\n";
        exit(1);
    }
    newnode_ssd->flag_ssd = flag_ssd;
    newnode_ssd->data_ssd = data_ssd;
    newnode_ssd->next_ssd = nullptr;
    newnode_ssd->sublist_ssd = nullptr;
    return newnode_ssd;
}

// Print GLL recursively
void printGLL_ssd(gll_ssd* head_ssd) {
    cout << "{";
    gll_ssd* temp_ssd = head_ssd;
    while (temp_ssd != nullptr) {
        if (temp_ssd->flag_ssd == 0) {
            // Atom
            cout << temp_ssd->data_ssd;
        } else {
            // Sublist
            printGLL_ssd(temp_ssd->sublist_ssd);
        }
        if (temp_ssd->next_ssd != nullptr)
            cout << ", ";
        temp_ssd = temp_ssd->next_ssd;
    }
    cout << "}";
}

// Destroy GLL recursively
void destroyGLL_ssd(gll_ssd* head_ssd) {
    gll_ssd* temp_ssd = head_ssd;
    while (temp_ssd != nullptr) {
        gll_ssd* nextnode_ssd = temp_ssd->next_ssd;
        if (temp_ssd->flag_ssd == 1)
            destroyGLL_ssd(temp_ssd->sublist_ssd);
        delete temp_ssd;
        temp_ssd = nextnode_ssd;
    }
}

// Build specific set:
// S = { p, q, { r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1 } }
gll_ssd* buildSet_ssd() {
    // Level 1: p, q, {sublist}
    gll_ssd* p_ssd   = getNode_ssd(0, "p");
    gll_ssd* q_ssd   = getNode_ssd(0, "q");
    gll_ssd* sub_ssd = getNode_ssd(1, "");   // this holds the inner big sublist

    p_ssd->next_ssd = q_ssd;
    q_ssd->next_ssd = sub_ssd;

    // Level 2 sublist: r, s, t, {}, {u,v}, w, x, {y,z}, a1, b1
    gll_ssd* r_ssd     = getNode_ssd(0, "r");
    gll_ssd* s_ssd     = getNode_ssd(0, "s");
    gll_ssd* t_ssd     = getNode_ssd(0, "t");
    gll_ssd* empty_ssd = getNode_ssd(1, "");  // {}
    gll_ssd* uv_ssd    = getNode_ssd(1, "");  // {u, v}
    gll_ssd* w_ssd     = getNode_ssd(0, "w");
    gll_ssd* x_ssd     = getNode_ssd(0, "x");
    gll_ssd* yz_ssd    = getNode_ssd(1, "");  // {y, z}
    gll_ssd* a1_ssd    = getNode_ssd(0, "a1");
    gll_ssd* b1_ssd    = getNode_ssd(0, "b1");

    // Link nodes at level 2
    sub_ssd->sublist_ssd = r_ssd;
    r_ssd->next_ssd      = s_ssd;
    s_ssd->next_ssd      = t_ssd;
    t_ssd->next_ssd      = empty_ssd;
    empty_ssd->next_ssd  = uv_ssd;
    uv_ssd->next_ssd     = w_ssd;
    w_ssd->next_ssd      = x_ssd;
    x_ssd->next_ssd      = yz_ssd;
    yz_ssd->next_ssd     = a1_ssd;
    a1_ssd->next_ssd     = b1_ssd;

    // Level 3 sublists
    // empty_ssd -> {} (empty set)
    empty_ssd->sublist_ssd = nullptr;

    // {u, v}
    gll_ssd* u_ssd = getNode_ssd(0, "u");
    gll_ssd* v_ssd = getNode_ssd(0, "v");
    uv_ssd->sublist_ssd = u_ssd;
    u_ssd->next_ssd     = v_ssd;

    // {y, z}
    gll_ssd* y_ssd = getNode_ssd(0, "y");
    gll_ssd* z_ssd = getNode_ssd(0, "z");
    yz_ssd->sublist_ssd = y_ssd;
    y_ssd->next_ssd     = z_ssd;

    // Return first node of main list
    return p_ssd;
}

int main() {
    gll_ssd* mySet_ssd = buildSet_ssd();

    cout << "The Generalized Linked List (Set) is:\n";
    printGLL_ssd(mySet_ssd);
    cout << endl;

    destroyGLL_ssd(mySet_ssd);
    return 0;
}
```

---

## 🧪 Example Output

```text
The Generalized Linked List (Set) is:
{p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1}}
```

This matches the required set:

\[
S = \{ p, q, \{ r, s, t, \{\}, \{u, v\}, w, x, \{y, z\}, a1, b1 \} \}
\]

---

## 🧠 Memory Visualization

### Node Layout (Conceptual)

- **Main level (S):**

```text
[p] -> [q] -> [sublist]
```

- `p` and `q` are **atoms** (`flag_ssd = 0`).
- `[sublist]` has `flag_ssd = 1` and `sublist_ssd` pointing to:

```text
[r] -> [s] -> [t] -> [empty] -> [uv] -> [w] -> [x] -> [yz] -> [a1] -> [b1]
```

- `empty` → `sublist_ssd = NULL` → `{}`  
- `uv` → `sublist_ssd` → `u -> v`  
- `yz` → `sublist_ssd` → `y -> z`  

This recursive linking allows arbitrary nesting of sets.

---

## ✅ Conclusion

In this assignment, you:

- Implemented a **Generalized Linked List (GLL)** to represent **nested sets**.
- Used:
  - `flag_ssd` to distinguish between **atoms** and **sublists**.
  - Recursive printing to generate **correct set notation** with `{}` and commas.
  - Recursive destruction to safely **free memory**.

This gives a strong foundation for representing **hierarchical data structures** like sets, trees, and nested lists using pointers in C++.

