# Heaps

## Overview

A heap is a specialized tree-based data structure that satisfies the heap property. It's a complete binary tree where each node follows a specific relationship with its children.

## Heap Property

### Min Heap
- Parent node value ≤ child node values
- Minimum element at root
- Used for ascending priority

### Max Heap
- Parent node value ≥ child node values
- Maximum element at root
- Used for descending priority

## Characteristics

- **Complete Binary Tree**: All levels filled except possibly the last (filled left to right)
- **Heap Property**: Parent-child relationship maintained
- **Array Representation**: Can be efficiently stored in array
- **Not Sorted**: Elements not in sorted order (unlike BST)

## Array Representation

For a node at index `i`:
- **Parent**: `(i - 1) // 2`
- **Left Child**: `2 * i + 1`
- **Right Child**: `2 * i + 2`

This allows O(1) access to parent and children without pointers.

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Insert    | O(log n) |
| Delete (root) | O(log n) |
| Get Min/Max | O(1) |
| Heapify | O(n) |
| Build Heap | O(n) |
| Heap Sort | O(n log n) |

## Space Complexity

O(n) where n is the number of elements

## Basic Operations

### 1. Insert (Heapify Up)
- Add element at end of array
- Bubble up to maintain heap property
- Compare with parent and swap if needed

### 2. Delete/Extract Min/Max (Heapify Down)
- Remove root element
- Replace with last element
- Bubble down to maintain heap property
- Compare with children and swap with smaller/larger

### 3. Peek
- Return root element without removing
- O(1) operation

## Min Heap Implementation

```python
class MinHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        """Get parent index"""
        return (i - 1) // 2
    
    def left_child(self, i):
        """Get left child index"""
        return 2 * i + 1
    
    def right_child(self, i):
        """Get right child index"""
        return 2 * i + 2
    
    def swap(self, i, j):
        """Swap elements at indices i and j"""
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    def insert(self, val):
        """Insert value - O(log n)"""
        self.heap.append(val)
        self._heapify_up(len(self.heap) - 1)
    
    def _heapify_up(self, i):
        """Bubble up element to maintain heap property"""
        parent = self.parent(i)
        
        if i > 0 and self.heap[i] < self.heap[parent]:
            self.swap(i, parent)
            self._heapify_up(parent)
    
    def extract_min(self):
        """Remove and return minimum element - O(log n)"""
        if not self.heap:
            raise IndexError("Heap is empty")
        
        if len(self.heap) == 1:
            return self.heap.pop()
        
        min_val = self.heap[0]
        self.heap[0] = self.heap.pop()
        self._heapify_down(0)
        
        return min_val
    
    def _heapify_down(self, i):
        """Bubble down element to maintain heap property"""
        min_index = i
        left = self.left_child(i)
        right = self.right_child(i)
        
        if left < len(self.heap) and self.heap[left] < self.heap[min_index]:
            min_index = left
        
        if right < len(self.heap) and self.heap[right] < self.heap[min_index]:
            min_index = right
        
        if min_index != i:
            self.swap(i, min_index)
            self._heapify_down(min_index)
    
    def peek(self):
        """Get minimum element - O(1)"""
        if not self.heap:
            raise IndexError("Heap is empty")
        return self.heap[0]
    
    def size(self):
        """Get heap size - O(1)"""
        return len(self.heap)
    
    def is_empty(self):
        """Check if heap is empty - O(1)"""
        return len(self.heap) == 0
    
    def build_heap(self, arr):
        """Build heap from array - O(n)"""
        self.heap = arr[:]
        # Start from last non-leaf node
        for i in range(len(self.heap) // 2 - 1, -1, -1):
            self._heapify_down(i)

# Example usage
if __name__ == "__main__":
    heap = MinHeap()
    
    # Insert elements
    values = [5, 3, 7, 1, 9, 4]
    for val in values:
        heap.insert(val)
    
    print(f"Heap array: {heap.heap}")
    print(f"Min element: {heap.peek()}")
    
    # Extract min
    print(f"Extracted: {heap.extract_min()}")
    print(f"Heap after extraction: {heap.heap}")
    
    # Build heap from array
    heap2 = MinHeap()
    heap2.build_heap([8, 5, 3, 1, 9, 6])
    print(f"Built heap: {heap2.heap}")
```

## Max Heap Implementation

```python
class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    def insert(self, val):
        """Insert value - O(log n)"""
        self.heap.append(val)
        self._heapify_up(len(self.heap) - 1)
    
    def _heapify_up(self, i):
        """Bubble up for max heap"""
        parent = self.parent(i)
        
        if i > 0 and self.heap[i] > self.heap[parent]:
            self.swap(i, parent)
            self._heapify_up(parent)
    
    def extract_max(self):
        """Remove and return maximum element - O(log n)"""
        if not self.heap:
            raise IndexError("Heap is empty")
        
        if len(self.heap) == 1:
            return self.heap.pop()
        
        max_val = self.heap[0]
        self.heap[0] = self.heap.pop()
        self._heapify_down(0)
        
        return max_val
    
    def _heapify_down(self, i):
        """Bubble down for max heap"""
        max_index = i
        left = self.left_child(i)
        right = self.right_child(i)
        
        if left < len(self.heap) and self.heap[left] > self.heap[max_index]:
            max_index = left
        
        if right < len(self.heap) and self.heap[right] > self.heap[max_index]:
            max_index = right
        
        if max_index != i:
            self.swap(i, max_index)
            self._heapify_down(max_index)
    
    def peek(self):
        """Get maximum element - O(1)"""
        if not self.heap:
            raise IndexError("Heap is empty")
        return self.heap[0]
```

## Using Python's heapq Module

Python provides a built-in heap implementation (min heap):

```python
import heapq

# Create empty heap
heap = []

# Insert elements - O(log n)
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappush(heap, 7)
heapq.heappush(heap, 1)

print(f"Heap: {heap}")  # [1, 3, 7, 5]

# Get minimum - O(1)
min_val = heap[0]
print(f"Min: {min_val}")

# Extract minimum - O(log n)
min_val = heapq.heappop(heap)
print(f"Extracted: {min_val}")
print(f"Heap: {heap}")

# Build heap from list - O(n)
arr = [8, 5, 3, 1, 9, 6]
heapq.heapify(arr)
print(f"Heapified: {arr}")

# Max heap using negative values
max_heap = []
for val in [5, 3, 7, 1, 9]:
    heapq.heappush(max_heap, -val)

max_val = -heapq.heappop(max_heap)
print(f"Max: {max_val}")

# Get n largest/smallest
arr = [1, 5, 3, 9, 7, 2]
print(f"3 largest: {heapq.nlargest(3, arr)}")
print(f"3 smallest: {heapq.nsmallest(3, arr)}")
```

## Heap Sort

Heap sort uses a heap to sort elements in O(n log n) time:

```python
def heap_sort(arr):
    """Sort array using heap sort - O(n log n)"""
    # Build max heap
    n = len(arr)
    
    # Heapify
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, n, i)
    
    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]  # Swap
        heapify_down(arr, i, 0)
    
    return arr

def heapify_down(arr, n, i):
    """Heapify down for max heap"""
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify_down(arr, n, largest)

# Test
arr = [12, 11, 13, 5, 6, 7]
print(f"Original: {arr}")
heap_sort(arr)
print(f"Sorted: {arr}")
```

## Common Heap Problems

### 1. Kth Largest Element
```python
import heapq

def find_kth_largest(nums, k):
    """Find kth largest element using min heap"""
    # Keep min heap of size k
    heap = nums[:k]
    heapq.heapify(heap)
    
    for num in nums[k:]:
        if num > heap[0]:
            heapq.heapreplace(heap, num)
    
    return heap[0]

# Alternative using nlargest
def find_kth_largest_v2(nums, k):
    return heapq.nlargest(k, nums)[-1]
```

### 2. Merge K Sorted Lists
```python
def merge_k_sorted_lists(lists):
    """Merge k sorted lists using heap"""
    import heapq
    
    heap = []
    result = []
    
    # Add first element of each list to heap
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst[0], i, 0))
    
    while heap:
        val, list_idx, element_idx = heapq.heappop(heap)
        result.append(val)
        
        # Add next element from same list
        if element_idx + 1 < len(lists[list_idx]):
            next_val = lists[list_idx][element_idx + 1]
            heapq.heappush(heap, (next_val, list_idx, element_idx + 1))
    
    return result
```

### 3. Top K Frequent Elements
```python
def top_k_frequent(nums, k):
    """Find k most frequent elements"""
    from collections import Counter
    import heapq
    
    count = Counter(nums)
    return heapq.nlargest(k, count.keys(), key=count.get)
```

### 4. Median Finder (Two Heaps)
```python
class MedianFinder:
    """Find median from data stream using two heaps"""
    
    def __init__(self):
        self.small = []  # Max heap (negated values)
        self.large = []  # Min heap
    
    def add_num(self, num):
        """Add number to data structure"""
        # Add to max heap (small)
        heapq.heappush(self.small, -num)
        
        # Balance: move largest from small to large
        if self.small and self.large and (-self.small[0] > self.large[0]):
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        # Balance sizes
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        if len(self.large) > len(self.small):
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)
    
    def find_median(self):
        """Get median"""
        if len(self.small) > len(self.large):
            return -self.small[0]
        return (-self.small[0] + self.large[0]) / 2.0
```

### 5. Task Scheduler
```python
def task_scheduler(tasks, n):
    """Schedule tasks with cooling period"""
    from collections import Counter
    import heapq
    
    count = Counter(tasks)
    max_heap = [-c for c in count.values()]
    heapq.heapify(max_heap)
    
    time = 0
    
    while max_heap:
        temp = []
        for _ in range(n + 1):
            if max_heap:
                temp.append(heapq.heappop(max_heap))
        
        for item in temp:
            if item + 1 < 0:
                heapq.heappush(max_heap, item + 1)
        
        time += (n + 1) if max_heap else len(temp)
    
    return time
```

## Priority Queue

Heaps are commonly used to implement priority queues:

```python
import heapq

class PriorityQueue:
    def __init__(self):
        self.heap = []
        self.counter = 0
    
    def push(self, item, priority):
        """Add item with priority"""
        heapq.heappush(self.heap, (priority, self.counter, item))
        self.counter += 1
    
    def pop(self):
        """Remove and return highest priority item"""
        if not self.heap:
            raise IndexError("Queue is empty")
        return heapq.heappop(self.heap)[2]
    
    def peek(self):
        """Get highest priority item without removing"""
        if not self.heap:
            raise IndexError("Queue is empty")
        return self.heap[0][2]
    
    def is_empty(self):
        return len(self.heap) == 0

# Usage
pq = PriorityQueue()
pq.push("task1", 3)
pq.push("task2", 1)
pq.push("task3", 2)

print(pq.pop())  # task2 (priority 1)
print(pq.pop())  # task3 (priority 2)
```

## Use Cases

1. **Priority Queues**: Task scheduling, event simulation
2. **Heap Sort**: Efficient sorting algorithm
3. **Graph Algorithms**: Dijkstra's shortest path, Prim's MST
4. **K Largest/Smallest**: Finding top K elements
5. **Median Maintenance**: Finding median in stream
6. **Merge K Sorted Arrays**: Efficient merging
7. **Operating Systems**: Process scheduling

## Advantages

- **Efficient Min/Max Access**: O(1) to get min/max
- **Efficient Insert/Delete**: O(log n) operations
- **Space Efficient**: Array representation, no pointers
- **Partial Ordering**: Don't need full sort

## Disadvantages

- **No Fast Search**: O(n) to search for arbitrary element
- **Not Fully Sorted**: Only partial ordering maintained
- **Limited Access**: Only root element efficiently accessible

## Practice Problems

1. **Kth Largest Element in Stream**: Maintain kth largest
2. **Reorganize String**: Rearrange to avoid adjacent duplicates
3. **Sliding Window Median**: Find median in sliding window
4. **Trapping Rain Water**: Using two heaps
5. **Meeting Rooms II**: Minimum meeting rooms needed

## Practice Resources

- LeetCode Heap Problems
- HackerRank Heap
- GeeksforGeeks Heap Data Structure

## Next Steps

- [Graphs](../../advanced/graphs/) - Using heaps for shortest path
- [Advanced Trees](../../advanced/advanced-trees/) - More complex tree structures
- Priority queue applications
