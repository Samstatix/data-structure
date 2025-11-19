# Trees

## Overview

A tree is a hierarchical data structure consisting of nodes connected by edges, with a single root node at the top. Each node can have zero or more child nodes, forming a parent-child relationship.

## Characteristics

- **Hierarchical Structure**: Root at top, leaves at bottom
- **No Cycles**: No path from a node back to itself
- **Connected**: Path exists between any two nodes
- **N-1 Edges**: Tree with N nodes has exactly N-1 edges

## Tree Terminology

- **Root**: Top node with no parent
- **Parent**: Node with child nodes
- **Child**: Node with a parent
- **Leaf**: Node with no children
- **Internal Node**: Node with at least one child
- **Subtree**: Tree formed by a node and its descendants
- **Depth**: Distance from root to node
- **Height**: Maximum depth of any node
- **Level**: Nodes at same depth
- **Degree**: Number of children of a node

## Types of Trees

### 1. Binary Tree
Each node has at most two children (left and right).

### 2. Binary Search Tree (BST)
- Left subtree contains nodes with values less than parent
- Right subtree contains nodes with values greater than parent
- Enables efficient searching

### 3. AVL Tree
- Self-balancing BST
- Height difference between left and right subtrees ≤ 1
- Guarantees O(log n) operations

### 4. Red-Black Tree
- Self-balancing BST with color property
- Guarantees O(log n) operations
- More relaxed balancing than AVL

### 5. Complete Binary Tree
All levels filled except possibly the last, which is filled left to right.

### 6. Full Binary Tree
Every node has either 0 or 2 children.

### 7. Perfect Binary Tree
All internal nodes have two children and all leaves at same level.

## Time Complexity

### Binary Search Tree (Balanced)
| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search    | O(log n) | O(n) unbalanced |
| Insert    | O(log n) | O(n) unbalanced |
| Delete    | O(log n) | O(n) unbalanced |

### AVL Tree (Always Balanced)
| Operation | Time Complexity |
|-----------|----------------|
| Search    | O(log n) |
| Insert    | O(log n) |
| Delete    | O(log n) |

## Space Complexity

O(n) where n is the number of nodes

## Tree Traversals

### 1. Inorder (Left-Root-Right)
- For BST: Visits nodes in ascending order
- Use: Get sorted order from BST

### 2. Preorder (Root-Left-Right)
- Root processed before subtrees
- Use: Copy tree, prefix expressions

### 3. Postorder (Left-Right-Root)
- Root processed after subtrees
- Use: Delete tree, postfix expressions

### 4. Level-order (BFS)
- Visit nodes level by level
- Use: Find level of node, shortest path

## Binary Tree Implementation

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class BinaryTree:
    def __init__(self):
        self.root = None
    
    def inorder(self, node):
        """Inorder traversal: Left-Root-Right"""
        if node:
            self.inorder(node.left)
            print(node.val, end=" ")
            self.inorder(node.right)
    
    def preorder(self, node):
        """Preorder traversal: Root-Left-Right"""
        if node:
            print(node.val, end=" ")
            self.preorder(node.left)
            self.preorder(node.right)
    
    def postorder(self, node):
        """Postorder traversal: Left-Right-Root"""
        if node:
            self.postorder(node.left)
            self.postorder(node.right)
            print(node.val, end=" ")
    
    def level_order(self):
        """Level-order traversal using queue"""
        if not self.root:
            return
        
        from collections import deque
        queue = deque([self.root])
        
        while queue:
            node = queue.popleft()
            print(node.val, end=" ")
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    def height(self, node):
        """Calculate height of tree"""
        if not node:
            return -1
        return 1 + max(self.height(node.left), self.height(node.right))
    
    def size(self, node):
        """Count number of nodes"""
        if not node:
            return 0
        return 1 + self.size(node.left) + self.size(node.right)
```

## Binary Search Tree Implementation

```python
class BSTNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        """Insert value - O(log n) average, O(n) worst"""
        self.root = self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        if not node:
            return BSTNode(val)
        
        if val < node.val:
            node.left = self._insert_recursive(node.left, val)
        elif val > node.val:
            node.right = self._insert_recursive(node.right, val)
        
        return node
    
    def search(self, val):
        """Search for value - O(log n) average"""
        return self._search_recursive(self.root, val)
    
    def _search_recursive(self, node, val):
        if not node or node.val == val:
            return node
        
        if val < node.val:
            return self._search_recursive(node.left, val)
        return self._search_recursive(node.right, val)
    
    def delete(self, val):
        """Delete value - O(log n) average"""
        self.root = self._delete_recursive(self.root, val)
    
    def _delete_recursive(self, node, val):
        if not node:
            return None
        
        if val < node.val:
            node.left = self._delete_recursive(node.left, val)
        elif val > node.val:
            node.right = self._delete_recursive(node.right, val)
        else:
            # Node to delete found
            # Case 1: No children or one child
            if not node.left:
                return node.right
            if not node.right:
                return node.left
            
            # Case 2: Two children
            # Find inorder successor (smallest in right subtree)
            min_node = self._find_min(node.right)
            node.val = min_node.val
            node.right = self._delete_recursive(node.right, min_node.val)
        
        return node
    
    def _find_min(self, node):
        """Find minimum value node"""
        while node.left:
            node = node.left
        return node
    
    def _find_max(self, node):
        """Find maximum value node"""
        while node.right:
            node = node.right
        return node
    
    def inorder(self, node, result=None):
        """Inorder traversal gives sorted order"""
        if result is None:
            result = []
        
        if node:
            self.inorder(node.left, result)
            result.append(node.val)
            self.inorder(node.right, result)
        
        return result

# Example usage
if __name__ == "__main__":
    bst = BinarySearchTree()
    
    # Insert
    values = [50, 30, 70, 20, 40, 60, 80]
    for val in values:
        bst.insert(val)
    
    # Inorder (sorted)
    print("Inorder:", bst.inorder(bst.root))  # [20, 30, 40, 50, 60, 70, 80]
    
    # Search
    print("Search 40:", bst.search(40) is not None)  # True
    print("Search 25:", bst.search(25) is not None)  # False
    
    # Delete
    bst.delete(30)
    print("After deleting 30:", bst.inorder(bst.root))
```

## AVL Tree Implementation

```python
class AVLNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.height = 1

class AVLTree:
    def get_height(self, node):
        """Get height of node"""
        if not node:
            return 0
        return node.height
    
    def get_balance(self, node):
        """Get balance factor"""
        if not node:
            return 0
        return self.get_height(node.left) - self.get_height(node.right)
    
    def rotate_right(self, z):
        """Right rotation"""
        y = z.left
        T3 = y.right
        
        y.right = z
        z.left = T3
        
        z.height = 1 + max(self.get_height(z.left), self.get_height(z.right))
        y.height = 1 + max(self.get_height(y.left), self.get_height(y.right))
        
        return y
    
    def rotate_left(self, z):
        """Left rotation"""
        y = z.right
        T2 = y.left
        
        y.left = z
        z.right = T2
        
        z.height = 1 + max(self.get_height(z.left), self.get_height(z.right))
        y.height = 1 + max(self.get_height(y.left), self.get_height(y.right))
        
        return y
    
    def insert(self, node, val):
        """Insert and balance - O(log n)"""
        # Standard BST insertion
        if not node:
            return AVLNode(val)
        
        if val < node.val:
            node.left = self.insert(node.left, val)
        else:
            node.right = self.insert(node.right, val)
        
        # Update height
        node.height = 1 + max(self.get_height(node.left), self.get_height(node.right))
        
        # Get balance factor
        balance = self.get_balance(node)
        
        # Left Left Case
        if balance > 1 and val < node.left.val:
            return self.rotate_right(node)
        
        # Right Right Case
        if balance < -1 and val > node.right.val:
            return self.rotate_left(node)
        
        # Left Right Case
        if balance > 1 and val > node.left.val:
            node.left = self.rotate_left(node.left)
            return self.rotate_right(node)
        
        # Right Left Case
        if balance < -1 and val < node.right.val:
            node.right = self.rotate_right(node.right)
            return self.rotate_left(node)
        
        return node
```

## Common Tree Problems

### 1. Maximum Depth
```python
def max_depth(root):
    """Find maximum depth of binary tree"""
    if not root:
        return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

### 2. Validate BST
```python
def is_valid_bst(root, min_val=float('-inf'), max_val=float('inf')):
    """Check if binary tree is valid BST"""
    if not root:
        return True
    
    if root.val <= min_val or root.val >= max_val:
        return False
    
    return (is_valid_bst(root.left, min_val, root.val) and
            is_valid_bst(root.right, root.val, max_val))
```

### 3. Lowest Common Ancestor
```python
def lca(root, p, q):
    """Find lowest common ancestor in BST"""
    if not root:
        return None
    
    if p.val < root.val and q.val < root.val:
        return lca(root.left, p, q)
    
    if p.val > root.val and q.val > root.val:
        return lca(root.right, p, q)
    
    return root
```

### 4. Level Order Traversal
```python
def level_order(root):
    """Level order traversal"""
    if not root:
        return []
    
    from collections import deque
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

### 5. Symmetric Tree
```python
def is_symmetric(root):
    """Check if tree is symmetric"""
    def is_mirror(left, right):
        if not left and not right:
            return True
        if not left or not right:
            return False
        return (left.val == right.val and
                is_mirror(left.left, right.right) and
                is_mirror(left.right, right.left))
    
    return is_mirror(root, root)
```

## Use Cases

1. **File Systems**: Directory structure
2. **DOM**: HTML/XML document structure
3. **Databases**: B-trees for indexing
4. **Compilers**: Abstract syntax trees (AST)
5. **Decision Making**: Decision trees
6. **Organization**: Organizational hierarchies
7. **AI**: Game trees (minimax algorithm)

## Advantages

- **Hierarchical Structure**: Natural representation of hierarchical data
- **Efficient Operations**: O(log n) for balanced trees
- **Sorted Order**: BST maintains sorted order
- **Range Queries**: Efficient range searches in BST

## Disadvantages

- **Complexity**: More complex than linear structures
- **Balancing Overhead**: Self-balancing trees require extra operations
- **Memory**: Extra memory for pointers
- **Unbalanced Risk**: BST can degrade to O(n) if unbalanced

## Practice Problems

1. **Binary Tree Paths**: Find all root-to-leaf paths
2. **Serialize and Deserialize**: Convert tree to/from string
3. **Kth Smallest Element**: Find kth smallest in BST
4. **Construct Tree**: From inorder and preorder traversals
5. **Sum Root to Leaf**: Calculate sum of all root-to-leaf numbers

## Practice Resources

- LeetCode Tree Problems
- HackerRank Trees
- GeeksforGeeks Tree Data Structure

## Next Steps

- [Heaps](../heaps/) - Special binary trees for priority operations
- [Advanced Trees](../../advanced/advanced-trees/) - Segment trees, Fenwick trees
- Graph algorithms using tree structures
