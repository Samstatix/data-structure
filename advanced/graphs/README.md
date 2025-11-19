# Graphs

## Overview

A graph is a non-linear data structure consisting of vertices (nodes) and edges that connect these vertices. Graphs are used to represent networks of communication, data organization, computational devices, and flow of computation.

## Characteristics

- **Vertices (Nodes)**: The fundamental units of the graph
- **Edges**: Connections between vertices
- **Flexibility**: Can represent many real-world relationships
- **Various Types**: Directed, undirected, weighted, unweighted

## Graph Types

### 1. Directed Graph (Digraph)
- Edges have direction (one-way)
- Edge (A, B) ≠ Edge (B, A)
- Example: Web pages (hyperlinks), Twitter followers

### 2. Undirected Graph
- Edges have no direction (two-way)
- Edge (A, B) = Edge (B, A)
- Example: Facebook friends, road networks

### 3. Weighted Graph
- Edges have weights/costs
- Used for optimization problems
- Example: Road networks with distances, flight routes with costs

### 4. Unweighted Graph
- All edges have equal weight (or weight = 1)
- Example: Social networks, maze representation

### 5. Cyclic Graph
- Contains at least one cycle (path from vertex back to itself)

### 6. Acyclic Graph
- Contains no cycles
- DAG (Directed Acyclic Graph) used in scheduling, dependencies

### 7. Connected Graph
- Path exists between every pair of vertices

### 8. Disconnected Graph
- Some vertices have no path between them

## Graph Representations

### 1. Adjacency Matrix
- 2D array of size V × V (V = number of vertices)
- matrix[i][j] = 1 if edge exists from i to j
- For weighted graphs, store weight instead of 1

**Advantages:**
- O(1) edge lookup
- Simple implementation
- Good for dense graphs

**Disadvantages:**
- O(V²) space complexity
- Adding vertex is expensive

```python
# Adjacency Matrix
class GraphMatrix:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = [[0] * vertices for _ in range(vertices)]
    
    def add_edge(self, u, v, weight=1):
        """Add edge from u to v"""
        self.graph[u][v] = weight
        # For undirected graph: self.graph[v][u] = weight
    
    def print_graph(self):
        """Print adjacency matrix"""
        for row in self.graph:
            print(row)
```

### 2. Adjacency List
- Array of lists
- Each vertex has a list of its adjacent vertices
- For weighted graphs, store (vertex, weight) pairs

**Advantages:**
- O(V + E) space complexity
- Efficient for sparse graphs
- Easy to iterate over neighbors

**Disadvantages:**
- O(V) edge lookup in worst case
- More complex than matrix

```python
# Adjacency List
class GraphList:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = [[] for _ in range(vertices)]
    
    def add_edge(self, u, v, weight=1):
        """Add edge from u to v"""
        self.graph[u].append((v, weight))
        # For undirected graph: self.graph[v].append((u, weight))
    
    def print_graph(self):
        """Print adjacency list"""
        for i in range(self.V):
            print(f"{i}: {self.graph[i]}")
```

## Graph Traversals

### 1. Breadth-First Search (BFS)
- Visits vertices level by level
- Uses queue
- Time: O(V + E), Space: O(V)

**Use Cases:**
- Shortest path in unweighted graph
- Level-order traversal
- Check if graph is bipartite

```python
from collections import deque

def bfs(graph, start):
    """BFS traversal"""
    visited = set()
    queue = deque([start])
    visited.add(start)
    result = []
    
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        for neighbor, _ in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result
```

### 2. Depth-First Search (DFS)
- Explores as far as possible along each branch
- Uses stack (or recursion)
- Time: O(V + E), Space: O(V)

**Use Cases:**
- Cycle detection
- Topological sorting
- Finding connected components
- Solving mazes

```python
def dfs_recursive(graph, vertex, visited=None, result=None):
    """DFS traversal (recursive)"""
    if visited is None:
        visited = set()
    if result is None:
        result = []
    
    visited.add(vertex)
    result.append(vertex)
    
    for neighbor, _ in graph[vertex]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited, result)
    
    return result

def dfs_iterative(graph, start):
    """DFS traversal (iterative)"""
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            result.append(vertex)
            
            for neighbor, _ in graph[vertex]:
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return result
```

## Shortest Path Algorithms

### 1. Dijkstra's Algorithm
- Finds shortest path from source to all vertices
- Works with non-negative weights
- Time: O((V + E) log V) with min-heap

```python
import heapq

def dijkstra(graph, start):
    """Dijkstra's shortest path algorithm"""
    distances = {i: float('inf') for i in range(len(graph))}
    distances[start] = 0
    pq = [(0, start)]  # (distance, vertex)
    
    while pq:
        curr_dist, u = heapq.heappop(pq)
        
        if curr_dist > distances[u]:
            continue
        
        for v, weight in graph[u]:
            distance = curr_dist + weight
            
            if distance < distances[v]:
                distances[v] = distance
                heapq.heappush(pq, (distance, v))
    
    return distances
```

### 2. Bellman-Ford Algorithm
- Finds shortest path from source
- Works with negative weights
- Detects negative cycles
- Time: O(V × E)

```python
def bellman_ford(graph, V, start):
    """Bellman-Ford algorithm"""
    distances = [float('inf')] * V
    distances[start] = 0
    
    # Relax all edges V-1 times
    for _ in range(V - 1):
        for u in range(V):
            for v, weight in graph[u]:
                if distances[u] != float('inf') and distances[u] + weight < distances[v]:
                    distances[v] = distances[u] + weight
    
    # Check for negative cycles
    for u in range(V):
        for v, weight in graph[u]:
            if distances[u] != float('inf') and distances[u] + weight < distances[v]:
                return None  # Negative cycle detected
    
    return distances
```

### 3. Floyd-Warshall Algorithm
- Finds shortest paths between all pairs
- Works with negative weights
- Time: O(V³)

```python
def floyd_warshall(graph, V):
    """Floyd-Warshall all-pairs shortest path"""
    dist = [[float('inf')] * V for _ in range(V)]
    
    # Distance to self is 0
    for i in range(V):
        dist[i][i] = 0
    
    # Initialize with edge weights
    for u in range(V):
        for v, weight in graph[u]:
            dist[u][v] = weight
    
    # Main algorithm
    for k in range(V):
        for i in range(V):
            for j in range(V):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist
```

## Minimum Spanning Tree

### 1. Kruskal's Algorithm
- Greedy algorithm using Union-Find
- Sorts edges by weight
- Time: O(E log E)

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True

def kruskal(edges, V):
    """Kruskal's MST algorithm"""
    edges.sort(key=lambda x: x[2])  # Sort by weight
    uf = UnionFind(V)
    mst = []
    total_weight = 0
    
    for u, v, weight in edges:
        if uf.union(u, v):
            mst.append((u, v, weight))
            total_weight += weight
    
    return mst, total_weight
```

### 2. Prim's Algorithm
- Greedy algorithm using min-heap
- Grows MST from starting vertex
- Time: O((V + E) log V)

```python
def prim(graph, V):
    """Prim's MST algorithm"""
    visited = [False] * V
    min_heap = [(0, 0, -1)]  # (weight, vertex, parent)
    mst = []
    total_weight = 0
    
    while min_heap:
        weight, u, parent = heapq.heappop(min_heap)
        
        if visited[u]:
            continue
        
        visited[u] = True
        if parent != -1:
            mst.append((parent, u, weight))
            total_weight += weight
        
        for v, w in graph[u]:
            if not visited[v]:
                heapq.heappush(min_heap, (w, v, u))
    
    return mst, total_weight
```

## Topological Sorting

For Directed Acyclic Graphs (DAG), topological sort gives linear ordering of vertices.

```python
def topological_sort_dfs(graph, V):
    """Topological sort using DFS"""
    visited = [False] * V
    stack = []
    
    def dfs(v):
        visited[v] = True
        for neighbor, _ in graph[v]:
            if not visited[neighbor]:
                dfs(neighbor)
        stack.append(v)
    
    for i in range(V):
        if not visited[i]:
            dfs(i)
    
    return stack[::-1]

def topological_sort_kahn(graph, V):
    """Topological sort using Kahn's algorithm (BFS)"""
    in_degree = [0] * V
    
    for u in range(V):
        for v, _ in graph[u]:
            in_degree[v] += 1
    
    queue = deque([i for i in range(V) if in_degree[i] == 0])
    result = []
    
    while queue:
        u = queue.popleft()
        result.append(u)
        
        for v, _ in graph[u]:
            in_degree[v] -= 1
            if in_degree[v] == 0:
                queue.append(v)
    
    return result if len(result) == V else None  # None if cycle exists
```

## Cycle Detection

```python
def has_cycle_directed(graph, V):
    """Detect cycle in directed graph"""
    WHITE, GRAY, BLACK = 0, 1, 2
    color = [WHITE] * V
    
    def dfs(v):
        color[v] = GRAY
        
        for neighbor, _ in graph[v]:
            if color[neighbor] == GRAY:
                return True
            if color[neighbor] == WHITE and dfs(neighbor):
                return True
        
        color[v] = BLACK
        return False
    
    for i in range(V):
        if color[i] == WHITE:
            if dfs(i):
                return True
    return False

def has_cycle_undirected(graph, V):
    """Detect cycle in undirected graph"""
    visited = [False] * V
    
    def dfs(v, parent):
        visited[v] = True
        
        for neighbor, _ in graph[v]:
            if not visited[neighbor]:
                if dfs(neighbor, v):
                    return True
            elif neighbor != parent:
                return True
        
        return False
    
    for i in range(V):
        if not visited[i]:
            if dfs(i, -1):
                return True
    return False
```

## Common Graph Problems

1. **Number of Islands**: Count connected components in grid
2. **Word Ladder**: Shortest transformation sequence
3. **Course Schedule**: Detect cycle in dependency graph
4. **Network Delay Time**: Find time for signal to reach all nodes
5. **Cheapest Flights**: Find cheapest path with K stops
6. **Graph Valid Tree**: Check if graph is a valid tree

## Use Cases

- **Social Networks**: Friends, followers, connections
- **Maps and Navigation**: Roads, cities, routes
- **Computer Networks**: Routers, connections
- **Dependency Resolution**: Package managers, build systems
- **Recommendation Systems**: User-item relationships
- **Web Crawling**: Pages and links
- **Compiler Design**: Control flow graphs

## Practice Resources

- LeetCode Graph Problems
- HackerRank Graphs
- GeeksforGeeks Graph Data Structure

## Next Steps

- [Tries](../tries/) - Specialized tree for strings
- [Advanced Trees](../advanced-trees/) - Segment trees, Fenwick trees
- Advanced graph algorithms (Network flow, Strongly connected components)
