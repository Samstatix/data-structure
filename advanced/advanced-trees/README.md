# Advanced Trees

## Overview

Advanced tree structures are specialized variations designed for specific use cases like range queries, database indexing, and efficient updates. These structures offer optimized performance for particular operations.

## Types of Advanced Trees

### 1. Segment Tree
A tree used for storing intervals or segments. Allows querying which segments contain a given point efficiently.

**Time Complexity:**
- Build: O(n)
- Query: O(log n)
- Update: O(log n)

**Use Cases:**
- Range sum/min/max queries
- Range updates
- Computational geometry

**Basic Implementation:**
```python
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.build(arr, 0, 0, self.n - 1)
    
    def build(self, arr, node, start, end):
        """Build segment tree - O(n)"""
        if start == end:
            self.tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            left_child = 2 * node + 1
            right_child = 2 * node + 2
            
            self.build(arr, left_child, start, mid)
            self.build(arr, right_child, mid + 1, end)
            
            self.tree[node] = self.tree[left_child] + self.tree[right_child]
    
    def query(self, node, start, end, l, r):
        """Query sum in range [l, r] - O(log n)"""
        if r < start or end < l:
            return 0
        
        if l <= start and end <= r:
            return self.tree[node]
        
        mid = (start + end) // 2
        left_sum = self.query(2 * node + 1, start, mid, l, r)
        right_sum = self.query(2 * node + 2, mid + 1, end, l, r)
        
        return left_sum + right_sum
    
    def update(self, node, start, end, idx, val):
        """Update value at index - O(log n)"""
        if start == end:
            self.tree[node] = val
        else:
            mid = (start + end) // 2
            left_child = 2 * node + 1
            right_child = 2 * node + 2
            
            if idx <= mid:
                self.update(left_child, start, mid, idx, val)
            else:
                self.update(right_child, mid + 1, end, idx, val)
            
            self.tree[node] = self.tree[left_child] + self.tree[right_child]

# Example usage
arr = [1, 3, 5, 7, 9, 11]
seg_tree = SegmentTree(arr)
print(seg_tree.query(0, 0, len(arr) - 1, 1, 3))  # Sum of arr[1:4] = 15
```

### 2. Fenwick Tree (Binary Indexed Tree)

A data structure that can efficiently update elements and calculate prefix sums.

**Time Complexity:**
- Build: O(n log n)
- Update: O(log n)
- Prefix Sum: O(log n)

**Use Cases:**
- Cumulative frequency tables
- Range sum queries
- Inversion counting

**Basic Implementation:**
```python
class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)
    
    def update(self, i, delta):
        """Add delta to element at index i - O(log n)"""
        i += 1  # 1-indexed
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)  # Add last set bit
    
    def prefix_sum(self, i):
        """Get sum of elements from 0 to i - O(log n)"""
        i += 1  # 1-indexed
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)  # Remove last set bit
        return total
    
    def range_sum(self, l, r):
        """Get sum in range [l, r] - O(log n)"""
        return self.prefix_sum(r) - (self.prefix_sum(l - 1) if l > 0 else 0)

# Example usage
ft = FenwickTree(6)
arr = [1, 3, 5, 7, 9, 11]
for i, val in enumerate(arr):
    ft.update(i, val)

print(ft.range_sum(1, 3))  # Sum of arr[1:4] = 15
```

### 3. B-Tree

A self-balancing tree designed for systems that read and write large blocks of data (databases, file systems).

**Characteristics:**
- Multiple keys per node
- All leaves at same level
- Nodes can have many children
- Optimized for disk access

**Time Complexity:**
- Search: O(log n)
- Insert: O(log n)
- Delete: O(log n)

**Use Cases:**
- Database indexing
- File systems
- Large datasets on disk

### 4. B+ Tree

Variation of B-Tree where all data is stored in leaves.

**Characteristics:**
- Internal nodes store only keys
- All data in leaf nodes
- Leaves linked as linked list
- Better for range queries

**Use Cases:**
- Database indexes (MySQL, PostgreSQL)
- File systems
- Range queries

### 5. Suffix Tree

Compressed trie of all suffixes of a string.

**Time Complexity:**
- Build: O(n) with Ukkonen's algorithm
- Search: O(m) for pattern of length m

**Use Cases:**
- Pattern matching
- Longest common substring
- DNA sequence analysis
- Text compression

### 6. Treap (Tree + Heap)

Binary search tree where each node has both a key and a priority (heap property on priorities).

**Characteristics:**
- BST property on keys
- Heap property on priorities
- Expected O(log n) operations
- Randomized structure

**Use Cases:**
- When BST needs randomization
- Priority-based BST

### 7. Splay Tree

Self-adjusting BST that moves accessed elements to root.

**Characteristics:**
- Recently accessed elements near root
- Amortized O(log n) operations
- No balance information stored

**Use Cases:**
- Cache implementation
- Frequently accessed data

### 8. K-D Tree

Space-partitioning tree for organizing points in k-dimensional space.

**Time Complexity:**
- Build: O(n log n)
- Nearest neighbor: O(log n) average

**Use Cases:**
- Nearest neighbor search
- Range search
- Computer graphics

## Comparison Table

| Tree Type | Build | Query | Update | Best For |
|-----------|-------|-------|--------|----------|
| Segment Tree | O(n) | O(log n) | O(log n) | Range queries with updates |
| Fenwick Tree | O(n log n) | O(log n) | O(log n) | Prefix sums, simpler than segment tree |
| B-Tree | O(n log n) | O(log n) | O(log n) | Database indexing, disk operations |
| B+ Tree | O(n log n) | O(log n) | O(log n) | Range queries, database indexes |
| Suffix Tree | O(n) | O(m) | - | Pattern matching, string problems |
| Treap | O(n log n) | O(log n) | O(log n) | Randomized BST operations |
| Splay Tree | - | O(log n)* | O(log n)* | Cache-like access patterns |
| K-D Tree | O(n log n) | O(log n) | - | Multidimensional search |

*Amortized time complexity

## Common Problems

### Range Sum Query
```python
class NumArray:
    def __init__(self, nums):
        self.seg_tree = SegmentTree(nums)
        self.n = len(nums)
    
    def sum_range(self, left, right):
        return self.seg_tree.query(0, 0, self.n - 1, left, right)
```

### Count of Smaller Numbers After Self
```python
def count_smaller(nums):
    """Count smaller elements to the right using Fenwick Tree"""
    # Coordinate compression
    sorted_nums = sorted(set(nums))
    ranks = {v: i for i, v in enumerate(sorted_nums)}
    
    ft = FenwickTree(len(sorted_nums))
    result = []
    
    for num in reversed(nums):
        rank = ranks[num]
        count = ft.prefix_sum(rank - 1) if rank > 0 else 0
        result.append(count)
        ft.update(rank, 1)
    
    return result[::-1]
```

## Use Cases by Domain

**Databases:**
- B-Trees, B+ Trees for indexing
- Segment trees for range queries

**String Processing:**
- Suffix trees for pattern matching
- Tries for prefix searches

**Computational Geometry:**
- K-D trees for spatial queries
- Segment trees for interval problems

**Competitive Programming:**
- Fenwick trees for quick implementation
- Segment trees for complex range queries

## Practice Problems

1. **Range Sum Query - Mutable**: Using segment tree or Fenwick tree
2. **Count of Smaller Numbers After Self**: Fenwick tree application
3. **Longest Repeated Substring**: Using suffix tree/array
4. **Merge K Sorted Lists**: Using heap-based tree
5. **Range Minimum Query**: Segment tree with min operation

## Practice Resources

- LeetCode Advanced Data Structures
- Codeforces EDU Section
- GeeksforGeeks Advanced Trees

## Next Steps

- Study specific tree types based on your use case
- Practice implementing from scratch
- Learn when to use which structure
- Explore applications in competitive programming and system design
