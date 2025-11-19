# Linked Lists

## Overview

A linked list is a linear data structure where elements (nodes) are not stored in contiguous memory locations. Each node contains data and a reference (link) to the next node in the sequence.

## Types of Linked Lists

1. **Singly Linked List**: Each node points to the next node
2. **Doubly Linked List**: Each node points to both next and previous nodes
3. **Circular Linked List**: Last node points back to the first node

## Characteristics

- **Dynamic Size**: Can grow or shrink during execution
- **Non-contiguous Memory**: Nodes can be scattered in memory
- **Sequential Access**: Must traverse from head to reach a node
- **Extra Memory**: Requires additional memory for storing pointers

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Access    | O(n)          |
| Search    | O(n)          |
| Insertion (at head) | O(1) |
| Insertion (at tail) | O(n) or O(1) with tail pointer |
| Insertion (at middle) | O(n) |
| Deletion (from head) | O(1) |
| Deletion (from tail) | O(n) |
| Deletion (from middle) | O(n) |

## Space Complexity

O(n) where n is the number of nodes

## Node Structure

Each node typically contains:
- **Data**: The value stored in the node
- **Next**: Pointer/reference to the next node
- **Prev** (for doubly linked lists): Pointer to the previous node

## Advantages

- **Dynamic Size**: No need to specify size in advance
- **Efficient Insertions/Deletions**: O(1) at head, no shifting required
- **Memory Efficient**: Allocate memory as needed
- **Implementation of Other Structures**: Useful for stacks, queues, graphs

## Disadvantages

- **Random Access Not Allowed**: Cannot directly access elements by index
- **Extra Memory**: Requires additional memory for pointers
- **Not Cache Friendly**: Nodes may be scattered in memory
- **Reverse Traversing**: Difficult in singly linked list

## Use Cases

1. **Dynamic Memory Allocation**: When size is unknown
2. **Implementation of Stacks and Queues**: Natural fit for these structures
3. **Undo Functionality**: In applications (linked list of states)
4. **Hash Tables**: For handling collisions (chaining)
5. **Graphs**: Adjacency list representation

## Example Implementation (Python)

```python
# Singly Linked List Implementation

class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def insert_at_beginning(self, data):
        """Insert a node at the beginning - O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
    
    def insert_at_end(self, data):
        """Insert a node at the end - O(n)"""
        new_node = Node(data)
        
        if not self.head:
            self.head = new_node
            return
        
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node
    
    def insert_after_node(self, prev_node, data):
        """Insert a node after a given node - O(1) if node is given"""
        if not prev_node:
            print("Previous node is not in the list")
            return
        
        new_node = Node(data)
        new_node.next = prev_node.next
        prev_node.next = new_node
    
    def delete_node(self, key):
        """Delete a node with given key - O(n)"""
        current = self.head
        
        # If head node holds the key
        if current and current.data == key:
            self.head = current.next
            current = None
            return
        
        # Search for the key
        prev = None
        while current and current.data != key:
            prev = current
            current = current.next
        
        # Key not found
        if not current:
            return
        
        # Unlink the node
        prev.next = current.next
        current = None
    
    def search(self, key):
        """Search for a node - O(n)"""
        current = self.head
        
        while current:
            if current.data == key:
                return True
            current = current.next
        
        return False
    
    def print_list(self):
        """Print the linked list"""
        current = self.head
        while current:
            print(current.data, end=" -> ")
            current = current.next
        print("None")
    
    def get_length(self):
        """Get the length of the list - O(n)"""
        count = 0
        current = self.head
        
        while current:
            count += 1
            current = current.next
        
        return count
    
    def reverse(self):
        """Reverse the linked list - O(n)"""
        prev = None
        current = self.head
        
        while current:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        
        self.head = prev

# Example usage
if __name__ == "__main__":
    ll = LinkedList()
    
    # Insert elements
    ll.insert_at_end(1)
    ll.insert_at_end(2)
    ll.insert_at_end(3)
    ll.insert_at_beginning(0)
    
    print("Original list:")
    ll.print_list()  # 0 -> 1 -> 2 -> 3 -> None
    
    # Search
    print(f"Search for 2: {ll.search(2)}")  # True
    print(f"Search for 5: {ll.search(5)}")  # False
    
    # Delete
    ll.delete_node(2)
    print("After deleting 2:")
    ll.print_list()  # 0 -> 1 -> 3 -> None
    
    # Length
    print(f"Length: {ll.get_length()}")  # 3
    
    # Reverse
    ll.reverse()
    print("After reversing:")
    ll.print_list()  # 3 -> 1 -> 0 -> None
```

## Doubly Linked List

```python
class DNode:
    def __init__(self, data):
        self.data = data
        self.next = None
        self.prev = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None
    
    def insert_at_beginning(self, data):
        """Insert at beginning - O(1)"""
        new_node = DNode(data)
        
        if self.head:
            new_node.next = self.head
            self.head.prev = new_node
        
        self.head = new_node
    
    def insert_at_end(self, data):
        """Insert at end - O(n)"""
        new_node = DNode(data)
        
        if not self.head:
            self.head = new_node
            return
        
        current = self.head
        while current.next:
            current = current.next
        
        current.next = new_node
        new_node.prev = current
    
    def print_forward(self):
        """Print list forward"""
        current = self.head
        while current:
            print(current.data, end=" <-> ")
            current = current.next
        print("None")
    
    def print_backward(self):
        """Print list backward"""
        current = self.head
        if not current:
            return
        
        # Go to the end
        while current.next:
            current = current.next
        
        # Print backward
        while current:
            print(current.data, end=" <-> ")
            current = current.prev
        print("None")
```

## Common Linked List Problems

1. **Reverse a Linked List**: Iterative and recursive approaches
2. **Detect a Loop**: Floyd's Cycle Detection Algorithm
3. **Find Middle Element**: Fast and slow pointer technique
4. **Merge Two Sorted Lists**: Merge operation
5. **Remove Nth Node from End**: Two-pointer approach
6. **Check if Palindrome**: Reverse half and compare

## Advanced Techniques

- **Floyd's Cycle Detection**: Use two pointers (slow and fast)
- **Runner Technique**: Use two pointers at different speeds
- **Dummy Head**: Simplify edge cases in insertion/deletion

## Practice Resources

- LeetCode Linked List Problems
- HackerRank Linked Lists
- GeeksforGeeks Linked List Data Structure

## Next Steps

After mastering linked lists, explore:
- [Stacks](../stacks/) - Can be implemented using linked lists
- [Queues](../queues/) - Can be implemented using linked lists
- [Trees](../../intermediate/trees/) - Extension of linked list concept
