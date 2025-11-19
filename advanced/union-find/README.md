# Union-Find (Disjoint Set)

## Overview

Union-Find, also known as Disjoint Set Union (DSU), is a data structure that keeps track of elements partitioned into disjoint (non-overlapping) sets. It provides near-constant-time operations to add new sets, merge existing sets, and determine whether elements are in the same set.

## Characteristics

- **Disjoint Sets**: Elements belong to exactly one set
- **Dynamic Connectivity**: Efficiently tracks connected components
- **Two Main Operations**: Union and Find
- **Near Constant Time**: With optimizations, operations are nearly O(1)

## Basic Operations

### 1. MakeSet
Create a new set containing a single element.

### 2. Find
Determine which set an element belongs to (find representative/root).

### 3. Union
Merge two sets into one.

## Time Complexity

| Operation | Without Optimization | With Path Compression | With Union by Rank | With Both |
|-----------|---------------------|----------------------|-------------------|-----------|
| MakeSet   | O(1)               | O(1)                | O(1)             | O(1)      |
| Find      | O(n)               | O(log n)            | O(log n)         | O(α(n))   |
| Union     | O(n)               | O(log n)            | O(log n)         | O(α(n))   |

**α(n)** is the inverse Ackermann function, which grows extremely slowly (< 5 for all practical values).

## Space Complexity

O(n) where n is the number of elements

## Basic Implementation

```python
class UnionFind:
    def __init__(self, n):
        """Initialize with n elements (0 to n-1)"""
        self.parent = list(range(n))
        self.size = n
    
    def find(self, x):
        """Find root of element x - O(n) worst case"""
        while x != self.parent[x]:
            x = self.parent[x]
        return x
    
    def union(self, x, y):
        """Unite sets containing x and y - O(n) worst case"""
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x != root_y:
            self.parent[root_x] = root_y
            return True
        return False
    
    def connected(self, x, y):
        """Check if x and y are in same set"""
        return self.find(x) == self.find(y)
```

## Optimized Implementation

### Path Compression
Makes tree flatter by pointing nodes directly to root during find operation.

### Union by Rank
Always attach smaller tree under root of larger tree.

```python
class UnionFindOptimized:
    def __init__(self, n):
        """Initialize with n elements"""
        self.parent = list(range(n))
        self.rank = [0] * n
        self.size = n
        self.count = n  # Number of disjoint sets
    
    def find(self, x):
        """Find with path compression - O(α(n))"""
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]
    
    def union(self, x, y):
        """Union by rank - O(α(n))"""
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x == root_y:
            return False
        
        # Union by rank
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
        
        self.count -= 1
        return True
    
    def connected(self, x, y):
        """Check if x and y are connected - O(α(n))"""
        return self.find(x) == self.find(y)
    
    def get_count(self):
        """Get number of disjoint sets"""
        return self.count

# Example usage
if __name__ == "__main__":
    uf = UnionFindOptimized(10)
    
    print(f"Initial sets: {uf.get_count()}")  # 10
    
    # Unite some elements
    uf.union(0, 1)
    uf.union(1, 2)
    uf.union(3, 4)
    
    print(f"Sets after unions: {uf.get_count()}")  # 7
    
    # Check connectivity
    print(f"0 and 2 connected: {uf.connected(0, 2)}")  # True
    print(f"0 and 3 connected: {uf.connected(0, 3)}")  # False
    
    # More unions
    uf.union(2, 4)
    print(f"0 and 3 connected now: {uf.connected(0, 3)}")  # True
```

## Union by Size

Alternative to union by rank - attach tree with fewer nodes to root of tree with more nodes.

```python
class UnionFindBySize:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n  # Size of each set
        self.count = n
    
    def find(self, x):
        """Find with path compression"""
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        """Union by size"""
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x == root_y:
            return False
        
        # Attach smaller to larger
        if self.size[root_x] < self.size[root_y]:
            self.parent[root_x] = root_y
            self.size[root_y] += self.size[root_x]
        else:
            self.parent[root_y] = root_x
            self.size[root_x] += self.size[root_y]
        
        self.count -= 1
        return True
    
    def get_size(self, x):
        """Get size of set containing x"""
        return self.size[self.find(x)]
```

## Common Problems

### 1. Number of Connected Components
```python
def count_components(n, edges):
    """Count connected components in undirected graph"""
    uf = UnionFindOptimized(n)
    
    for u, v in edges:
        uf.union(u, v)
    
    return uf.get_count()

# Test
edges = [[0, 1], [1, 2], [3, 4]]
print(count_components(5, edges))  # 2
```

### 2. Detect Cycle in Undirected Graph
```python
def has_cycle(n, edges):
    """Detect if undirected graph has cycle"""
    uf = UnionFindOptimized(n)
    
    for u, v in edges:
        if uf.connected(u, v):
            return True  # Cycle detected
        uf.union(u, v)
    
    return False

# Test
edges = [[0, 1], [1, 2], [2, 0]]
print(has_cycle(3, edges))  # True
```

### 3. Number of Islands
```python
def num_islands(grid):
    """Count number of islands in 2D grid"""
    if not grid or not grid[0]:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    uf = UnionFindOptimized(rows * cols)
    count = 0
    
    def get_index(r, c):
        return r * cols + c
    
    # Count land cells and union adjacent lands
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                
                # Check right and down neighbors
                for dr, dc in [(0, 1), (1, 0)]:
                    nr, nc = r + dr, c + dc
                    if (nr < rows and nc < cols and 
                        grid[nr][nc] == '1'):
                        if uf.union(get_index(r, c), get_index(nr, nc)):
                            count -= 1
    
    return count
```

### 4. Friend Circles
```python
def find_circle_num(is_connected):
    """Find number of friend circles"""
    n = len(is_connected)
    uf = UnionFindOptimized(n)
    
    for i in range(n):
        for j in range(i + 1, n):
            if is_connected[i][j] == 1:
                uf.union(i, j)
    
    return uf.get_count()
```

### 5. Accounts Merge
```python
def accounts_merge(accounts):
    """Merge accounts with common emails"""
    from collections import defaultdict
    
    email_to_id = {}
    email_to_name = {}
    uf = UnionFindOptimized(len(accounts))
    
    # Map emails to account IDs
    for i, account in enumerate(accounts):
        name = account[0]
        for email in account[1:]:
            email_to_name[email] = name
            
            if email in email_to_id:
                uf.union(i, email_to_id[email])
            else:
                email_to_id[email] = i
    
    # Group emails by root account
    root_to_emails = defaultdict(list)
    for email, acc_id in email_to_id.items():
        root = uf.find(acc_id)
        root_to_emails[root].append(email)
    
    # Build result
    result = []
    for root, emails in root_to_emails.items():
        name = accounts[root][0]
        result.append([name] + sorted(emails))
    
    return result
```

### 6. Redundant Connection
```python
def find_redundant_connection(edges):
    """Find edge that creates cycle in tree"""
    n = len(edges)
    uf = UnionFindOptimized(n + 1)
    
    for u, v in edges:
        if uf.connected(u, v):
            return [u, v]  # This edge creates cycle
        uf.union(u, v)
    
    return []
```

### 7. Smallest String With Swaps
```python
def smallest_string_with_swaps(s, pairs):
    """Get smallest string by swapping pairs"""
    from collections import defaultdict
    
    n = len(s)
    uf = UnionFindOptimized(n)
    
    # Union all swappable indices
    for i, j in pairs:
        uf.union(i, j)
    
    # Group indices by root
    groups = defaultdict(list)
    for i in range(n):
        root = uf.find(i)
        groups[root].append(i)
    
    # Sort characters in each group
    result = list(s)
    for indices in groups.values():
        chars = sorted([s[i] for i in indices])
        indices.sort()
        for i, char in zip(indices, chars):
            result[i] = char
    
    return ''.join(result)
```

## Kruskal's MST Algorithm

Union-Find is essential for Kruskal's Minimum Spanning Tree algorithm:

```python
def kruskal_mst(n, edges):
    """Find MST using Kruskal's algorithm"""
    # edges: [(weight, u, v), ...]
    edges.sort()  # Sort by weight
    
    uf = UnionFindOptimized(n)
    mst = []
    total_weight = 0
    
    for weight, u, v in edges:
        if not uf.connected(u, v):
            uf.union(u, v)
            mst.append((u, v, weight))
            total_weight += weight
            
            if len(mst) == n - 1:
                break
    
    return mst, total_weight
```

## Use Cases

1. **Network Connectivity**: Determine if nodes are connected
2. **Kruskal's Algorithm**: Minimum spanning tree
3. **Least Common Ancestor**: In trees
4. **Image Processing**: Connected component labeling
5. **Social Networks**: Finding communities/groups
6. **Percolation Theory**: Study of connectivity
7. **Clustering**: Group similar elements

## Advantages

- **Nearly Constant Time**: With optimizations, operations are O(α(n))
- **Simple Implementation**: Easy to code and understand
- **Space Efficient**: O(n) space
- **Dynamic**: Supports online queries

## Disadvantages

- **Cannot Disconnect**: Once united, sets cannot be separated
- **No Set Members**: Cannot easily list all members of a set
- **Limited Operations**: Only supports union and find
- **Not for Directed Graphs**: Works only for undirected connectivity

## Optimizations Summary

1. **Path Compression**: Make tree flat during find
2. **Union by Rank**: Attach smaller tree to larger
3. **Union by Size**: Attach tree with fewer nodes
4. **Both Optimizations**: Achieve O(α(n)) complexity

## Variations

### Weighted Union-Find
Track weights/distances from parent.

### Persistent Union-Find
Maintain history of operations.

### Online Union-Find
Process queries in real-time.

## Practice Problems

1. **Friend Circles**: Count number of friend groups
2. **Number of Provinces**: Connected components
3. **Redundant Connection**: Find edge creating cycle
4. **Accounts Merge**: Merge accounts with same emails
5. **Regions Cut by Slashes**: Count regions in grid
6. **Satisfiability of Equality Equations**: Check if equations satisfiable
7. **Most Stones Removed**: Maximum stones that can be removed

## Practice Resources

- LeetCode Union-Find Problems
- HackerRank Disjoint Set
- GeeksforGeeks Union-Find

## Next Steps

- [Graphs](../graphs/) - Apply Union-Find to graph problems
- [Advanced Trees](../advanced-trees/) - Other tree structures
- Minimum spanning tree algorithms
- Advanced graph algorithms
