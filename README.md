# Data Structures - Basic to Advanced

A comprehensive guide to data structures, organized from basic to advanced concepts. This repository includes implementations, explanations, and complexity analysis for each data structure.

## 📚 Table of Contents

- [Introduction](#introduction)
- [Learning Path](#learning-path)
- [Basic Data Structures](#basic-data-structures)
- [Intermediate Data Structures](#intermediate-data-structures)
- [Advanced Data Structures](#advanced-data-structures)
- [Contributing](#contributing)

## Introduction

Understanding data structures is fundamental to computer science and programming. This repository provides a structured learning path from basic to advanced data structures, helping you build a solid foundation and progress to more complex concepts.

## Learning Path

Follow this recommended learning path for optimal understanding:

1. **Foundation (Basic)** → Start here if you're new to data structures
2. **Intermediate** → Build upon basic concepts
3. **Advanced** → Master complex data structures and their applications

## Basic Data Structures

Essential data structures every programmer should know:

### 1. Arrays
- **Description**: Contiguous memory storage for elements of the same type
- **Time Complexity**: 
  - Access: O(1)
  - Search: O(n)
  - Insertion: O(n)
  - Deletion: O(n)
- **Use Cases**: When you need fast access by index

### 2. Linked Lists
- **Description**: Linear data structure where elements are linked using pointers
- **Types**: Singly Linked List, Doubly Linked List, Circular Linked List
- **Time Complexity**:
  - Access: O(n)
  - Search: O(n)
  - Insertion: O(1) at head/tail
  - Deletion: O(1) at head/tail
- **Use Cases**: Dynamic memory allocation, implementation of stacks/queues

### 3. Stacks
- **Description**: LIFO (Last In First Out) data structure
- **Time Complexity**:
  - Push: O(1)
  - Pop: O(1)
  - Peek: O(1)
- **Use Cases**: Function call stack, undo mechanisms, expression evaluation

### 4. Queues
- **Description**: FIFO (First In First Out) data structure
- **Types**: Simple Queue, Circular Queue, Priority Queue, Deque
- **Time Complexity**:
  - Enqueue: O(1)
  - Dequeue: O(1)
  - Peek: O(1)
- **Use Cases**: Task scheduling, breadth-first search, buffering

## Intermediate Data Structures

Building on basic concepts:

### 5. Hash Tables
- **Description**: Key-value pair storage using hash functions
- **Time Complexity** (Average):
  - Search: O(1)
  - Insertion: O(1)
  - Deletion: O(1)
- **Use Cases**: Caching, database indexing, counting frequencies

### 6. Trees
- **Description**: Hierarchical data structure with root and child nodes
- **Types**: Binary Tree, Binary Search Tree (BST), AVL Tree, Red-Black Tree
- **Time Complexity** (BST):
  - Search: O(log n) average, O(n) worst
  - Insertion: O(log n) average, O(n) worst
  - Deletion: O(log n) average, O(n) worst
- **Use Cases**: File systems, DOM, organizational hierarchies

### 7. Heaps
- **Description**: Complete binary tree satisfying heap property
- **Types**: Min Heap, Max Heap
- **Time Complexity**:
  - Insert: O(log n)
  - Delete: O(log n)
  - Get Min/Max: O(1)
- **Use Cases**: Priority queues, heap sort, finding k largest/smallest elements

## Advanced Data Structures

Complex data structures for advanced applications:

### 8. Graphs
- **Description**: Collection of nodes (vertices) connected by edges
- **Types**: Directed, Undirected, Weighted, Unweighted
- **Representations**: Adjacency Matrix, Adjacency List
- **Common Algorithms**: BFS, DFS, Dijkstra's, Kruskal's, Prim's
- **Use Cases**: Social networks, maps, network routing

### 9. Tries (Prefix Trees)
- **Description**: Tree-like structure for storing strings efficiently
- **Time Complexity**:
  - Search: O(m) where m is key length
  - Insert: O(m)
  - Delete: O(m)
- **Use Cases**: Autocomplete, spell checkers, IP routing

### 10. Advanced Trees
- **B-Trees**: Self-balancing tree for databases and file systems
- **B+ Trees**: Variation of B-tree with data in leaves
- **Segment Trees**: For range queries
- **Fenwick Trees (Binary Indexed Trees)**: For prefix sums
- **Use Cases**: Databases, file systems, computational geometry

### 11. Disjoint Set (Union-Find)
- **Description**: Tracks set of elements partitioned into disjoint sets
- **Operations**: Find, Union
- **Use Cases**: Network connectivity, Kruskal's algorithm

### 12. Bloom Filters
- **Description**: Probabilistic data structure for set membership
- **Use Cases**: Cache filtering, spell checking, database query optimization

## Directory Structure

```
data-structure/
├── README.md
├── basic/
│   ├── arrays/
│   ├── linked-lists/
│   ├── stacks/
│   └── queues/
├── intermediate/
│   ├── hash-tables/
│   ├── trees/
│   └── heaps/
└── advanced/
    ├── graphs/
    ├── tries/
    ├── advanced-trees/
    ├── union-find/
    └── bloom-filters/
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Resources

- **Books**: "Introduction to Algorithms" by CLRS, "Data Structures and Algorithms" by Aho, Hopcroft, and Ullman
- **Online**: GeeksforGeeks, LeetCode, HackerRank

## License

This repository is for educational purposes.