# Breadth-First Search (BFS)

## Definition

**Breadth-First Search (BFS)** is a graph/tree traversal algorithm that explores nodes **level by level**.

BFS uses a **Queue (FIFO — First In, First Out)** to keep track of the nodes that need to be visited.

The main idea is:

```text
Visit Current Node
       ↓
Visit All Neighbors
       ↓
Move to Next Level
       ↓
Repeat
```

---

# Core Idea

```text
Level by Level
```

Example:

```text
        1
       / \
      2   3
     / \   \
    4   5   6
```

BFS traversal:

```text
1 → 2 → 3 → 4 → 5 → 6
```

It first visits:

```text
Level 0 → 1
```

Then:

```text
Level 1 → 2, 3
```

Then:

```text
Level 2 → 4, 5, 6
```

---

# Why Queue?

BFS uses a **Queue** because a queue follows:

```text
FIFO
First In → First Out
```

Example:

```text
Queue:

Front                    Rear
 ↓                         ↓
[2] [3] [4] [5]
 ↑
Remove first
```

The node that enters the queue first is processed first.

---

# BFS Working Structure

Consider:

```text
        1
       / \
      2   3
     / \
    4   5
```

Initially:

```text
Queue = [1]
```

Visit `1`:

```text
Queue = [2, 3]
```

Visit `2`:

```text
Queue = [3, 4, 5]
```

Visit `3`:

```text
Queue = [4, 5]
```

Visit `4`:

```text
Queue = [5]
```

Visit `5`:

```text
Queue = []
```

Traversal:

```text
1 → 2 → 3 → 4 → 5
```

---

# Step 1 — Start Node

Add the starting node to the queue.

```python
queue = deque([start])
```

---

# Step 2 — Mark as Visited

For graphs, we usually maintain a `visited` set.

```python
visited = set()
visited.add(start)
```

This prevents visiting the same node repeatedly.

---

# Step 3 — Remove from Queue

Take the first node from the queue:

```python
node = queue.popleft()
```

Because the queue follows FIFO.

---

# Step 4 — Process the Node

Perform whatever operation the problem requires.

For example:

```python
result.append(node)
```

---

# Step 5 — Add Unvisited Neighbors

Check every neighbor:

```python
for neighbor in graph[node]:

    if neighbor not in visited:

        visited.add(neighbor)
        queue.append(neighbor)
```

---

# Basic BFS Template

```python
from collections import deque

def bfs(graph, start):

    queue = deque([start])
    visited = set([start])

    while queue:

        node = queue.popleft()

        print(node)

        for neighbor in graph[node]:

            if neighbor not in visited:

                visited.add(neighbor)
                queue.append(neighbor)
```

---

# Example

Graph:

```python
graph = {
    1: [2, 3],
    2: [4, 5],
    3: [6],
    4: [],
    5: [],
    6: []
}
```

Run:

```python
bfs(graph, 1)
```

Output:

```text
1 2 3 4 5 6
```

---

# BFS Using a Tree

For a binary tree, BFS is also called **Level Order Traversal**.

Example:

```text
        1
       / \
      2   3
     / \ / \
    4  5 6  7
```

BFS:

```text
1
↓
2 3
↓
4 5 6 7
```

Result:

```text
[1, 2, 3, 4, 5, 6, 7]
```

---

# Binary Tree BFS Code

```python
from collections import deque

def bfs(root):

    if not root:
        return []

    queue = deque([root])
    result = []

    while queue:

        node = queue.popleft()

        result.append(node.val)

        if node.left:
            queue.append(node.left)

        if node.right:
            queue.append(node.right)

    return result
```

---

# Level Order Traversal

Sometimes the problem requires each level separately.

Example:

```text
        1
       / \
      2   3
     / \   \
    4   5   6
```

Expected:

```python
[
    [1],
    [2, 3],
    [4, 5, 6]
]
```

Use the current queue size to process one complete level.

```python
from collections import deque

def level_order(root):

    if not root:
        return []

    queue = deque([root])
    result = []

    while queue:

        level = []

        for _ in range(len(queue)):

            node = queue.popleft()

            level.append(node.val)

            if node.left:
                queue.append(node.left)

            if node.right:
                queue.append(node.right)

        result.append(level)

    return result
```

---

# Why `len(queue)`?

This is an important BFS concept.

Suppose:

```text
Queue = [2, 3]
```

At the beginning of the level:

```python
len(queue) = 2
```

So we process exactly these two nodes.

While processing them, their children may be added:

```text
Queue initially:
[2, 3]

Process 2 → add 4, 5
Process 3 → add 6

Queue:
[4, 5, 6]
```

The newly added nodes belong to the **next level**.

Therefore:

```python
for _ in range(len(queue)):
```

captures the current level.

---

# BFS in a Grid

BFS is frequently used in matrix/grid problems.

Example:

```text
0 0 1
0 0 0
1 0 0
```

We can move in four directions:

```python
directions = [
    (1, 0),
    (-1, 0),
    (0, 1),
    (0, -1)
]
```

For each cell:

```python
for dr, dc in directions:

    nr = r + dr
    nc = c + dc
```

Then check whether the new position is valid.

---

# Grid BFS Template

```python
from collections import deque

def bfs(grid, r, c):

    rows = len(grid)
    cols = len(grid[0])

    queue = deque([(r, c)])
    visited = {(r, c)}

    directions = [
        (1, 0),
        (-1, 0),
        (0, 1),
        (0, -1)
    ]

    while queue:

        r, c = queue.popleft()

        for dr, dc in directions:

            nr = r + dr
            nc = c + dc

            if (
                0 <= nr < rows
                and 0 <= nc < cols
                and (nr, nc) not in visited
            ):

                visited.add((nr, nc))
                queue.append((nr, nc))
```

---

# BFS for Shortest Path

One of the most important applications of BFS is finding the **shortest path in an unweighted graph**.

Example:

```text
A → B → D
 \      ↑
  → C →─
```

Starting from `A`:

```text
Level 0 → A
Level 1 → B, C
Level 2 → D
```

Therefore the shortest distance from `A` to `D` is:

```text
2
```

BFS works because it explores nodes in increasing order of distance from the source.

---

# BFS with Distance

```python
from collections import deque

def bfs(graph, start):

    queue = deque([(start, 0)])
    visited = {start}

    while queue:

        node, distance = queue.popleft()

        print(node, distance)

        for neighbor in graph[node]:

            if neighbor not in visited:

                visited.add(neighbor)

                queue.append(
                    (neighbor, distance + 1)
                )
```

---

# BFS Dry Run

Graph:

```text
        1
       / \
      2   3
     / \
    4   5
```

Start:

```text
1
```

### Step 1

```text
Queue = [1]
Visited = {1}
```

Remove `1`:

```text
Queue = []
```

Neighbors:

```text
2, 3
```

Add them:

```text
Queue = [2, 3]
Visited = {1, 2, 3}
```

---

### Step 2

Remove `2`:

```text
Queue = [3]
```

Neighbors:

```text
4, 5
```

Add them:

```text
Queue = [3, 4, 5]
```

---

### Step 3

Remove `3`:

```text
Queue = [4, 5]
```

No new neighbors.

---

### Step 4

Remove `4`:

```text
Queue = [5]
```

---

### Step 5

Remove `5`:

```text
Queue = []
```

Final traversal:

```text
1 → 2 → 3 → 4 → 5
```

---

# Important BFS Template

For most graph BFS problems, remember:

```python
from collections import deque

queue = deque([start])
visited = {start}

while queue:

    node = queue.popleft()

    for neighbor in graph[node]:

        if neighbor not in visited:

            visited.add(neighbor)
            queue.append(neighbor)
```

---

# Time Complexity

For a graph:

```text
O(V + E)
```

Where:

```text
V = Number of vertices/nodes
E = Number of edges
```

Every node is processed once and every edge is considered.

---

# Space Complexity

```text
O(V)
```

Space is required for:

```text
Queue
+
Visited Set
```

In the worst case, the queue and visited structure can contain many nodes.

---

# BFS vs DFS

| Feature        | BFS                   | DFS                      |
| -------------- | --------------------- | ------------------------ |
| Full Form      | Breadth-First Search  | Depth-First Search       |
| Main Structure | Queue                 | Stack / Recursion        |
| Traversal      | Level by level        | Depth first              |
| Shortest Path  | Yes*                  | Not guaranteed           |
| Space          | O(V)                  | O(V)                     |
| Common Use     | Shortest path, levels | Backtracking, components |

`*` BFS finds shortest paths in **unweighted graphs**.

---

# When to Use BFS?

Think about BFS when the problem involves:

```text
Shortest path in an unweighted graph
        ↓
Level-by-level traversal
        ↓
Minimum number of steps
        ↓
Nearest / closest node
        ↓
Binary tree level order
        ↓
Grid shortest path
        ↓
Multi-source expansion
```

---

# How to Recognize BFS?

Ask yourself:

```text
"Do I need to explore level by level?"
```

or:

```text
"Do I need the minimum number of steps?"
```

or:

```text
"Is this an unweighted shortest-path problem?"
```

If yes, **BFS should be one of your first approaches to consider.**

---

# Common BFS Problems

BFS is commonly used for:

```text
1. Level Order Traversal
2. Shortest Path in Unweighted Graph
3. Number of Islands
4. Rotting Oranges
5. Word Ladder
6. Binary Tree Right Side View
7. Walls and Gates
8. 01 Matrix
9. Minimum Knight Moves
10. Open the Lock
```

---

# Common Mistakes

## 1. Forgetting `visited`

Without a visited set, a graph containing cycles can cause repeated processing.

```python
visited = {start}
```

---

## 2. Marking Visited Too Late

Prefer marking a node when you **add it to the queue**:

```python
visited.add(neighbor)
queue.append(neighbor)
```

This prevents the same node from being added multiple times.

---

## 3. Using `pop(0)`

Avoid:

```python
queue.pop(0)
```

For Python lists, this is inefficient.

Use:

```python
from collections import deque

queue.popleft()
```

---

## 4. Forgetting Grid Boundaries

For a grid:

```python
0 <= nr < rows
0 <= nc < cols
```

must be checked before accessing the cell.

---

# Mental Model

Remember BFS like a wave:

```text
             Start
               ↓
          ┌────┴────┐
          ↓         ↓
         Level 1  Level 1
          ↓         ↓
       ┌──┴──┐   ┌──┴──┐
       ↓     ↓   ↓     ↓
     Level 2      Level 2
```

It expands outward from the starting point.

---

# Key Takeaway

BFS always follows:

```text
Start
 ↓
Queue
 ↓
Pop Front
 ↓
Visit Neighbors
 ↓
Add Unvisited Neighbors
 ↓
Repeat
```

The most important template is:

```python
queue = deque([start])
visited = {start}

while queue:

    node = queue.popleft()

    for neighbor in graph[node]:

        if neighbor not in visited:

            visited.add(neighbor)
            queue.append(neighbor)
```

---

# Quick Revision

```text
Algorithm: Breadth-First Search

Data Structure = Queue
Pattern        = FIFO

Graph Time     = O(V + E)
Space          = O(V)

Main Uses:
- Level Order Traversal
- Shortest Path
- Minimum Steps
- Nearest Node
- Grid BFS

Core Pattern:

Queue
  ↓
Pop
  ↓
Process
  ↓
Neighbors
  ↓
Visited?
  ↓
Add to Queue
```

## One-Line Memory Trick

```text
BFS = Queue + Level by Level + Shortest Path
```
