# Queues

## Overview

A queue is a linear data structure that follows the **FIFO (First In First Out)** principle. The first element added to the queue will be the first one to be removed.

Think of it like a line of people waiting - the first person in line is the first one served.

## Characteristics

- **FIFO Order**: First In, First Out
- **Two End Operations**: Insertion at rear, deletion from front
- **Limited Access**: Can only access front and rear elements
- **Dynamic Size**: Can grow and shrink as needed

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Enqueue (Insert at rear) | O(1) |
| Dequeue (Remove from front) | O(1) |
| Front/Peek | O(1) |
| Rear | O(1) |
| isEmpty | O(1) |
| Search | O(n) |

## Space Complexity

O(n) where n is the number of elements

## Types of Queues

### 1. Simple Queue
Basic FIFO queue with insertion at rear and deletion from front.

### 2. Circular Queue
Last position connects back to first position, utilizing space efficiently.

### 3. Priority Queue
Elements are dequeued based on priority rather than insertion order.

### 4. Deque (Double-Ended Queue)
Insertion and deletion possible at both ends.

## Basic Operations

### 1. Enqueue
Add an element to the rear of the queue.

### 2. Dequeue
Remove and return the front element from the queue.

### 3. Front/Peek
Return the front element without removing it.

### 4. Rear
Return the rear element without removing it.

### 5. isEmpty
Check if the queue is empty.

### 6. Size
Get the number of elements in the queue.

## Implementation Methods

Queues can be implemented using:
1. **Arrays**: Fixed or circular implementation
2. **Linked Lists**: Dynamic size, efficient operations
3. **Stacks**: Using two stacks

## Advantages

- **Ordered Processing**: Maintains FIFO order
- **Fair Scheduling**: First come, first served
- **Efficient Operations**: All basic operations are O(1)
- **Buffering**: Natural fit for buffering data

## Disadvantages

- **Limited Access**: Can only access front and rear
- **Not Searchable**: Requires dequeuing to search
- **Array Implementation**: May waste space or require resizing

## Use Cases

1. **Task Scheduling**: CPU scheduling, job queues
2. **Breadth-First Search (BFS)**: Graph traversal
3. **Buffer Management**: IO buffers, printer queues
4. **Request Handling**: Web server request queues
5. **Message Queues**: Asynchronous communication
6. **Resource Sharing**: Print spooling, disk scheduling

## Example Implementation (Python)

```python
# Queue implementation using list

class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, item):
        """Add item to rear - O(1)"""
        self.items.append(item)
    
    def dequeue(self):
        """Remove and return front item - O(n) due to list.pop(0)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items.pop(0)
    
    def front(self):
        """Return front item without removing - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items[0]
    
    def rear(self):
        """Return rear item without removing - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items[-1]
    
    def is_empty(self):
        """Check if queue is empty - O(1)"""
        return len(self.items) == 0
    
    def size(self):
        """Get the size of the queue - O(1)"""
        return len(self.items)
    
    def __str__(self):
        """String representation"""
        return f"Front -> {self.items} <- Rear"

# Example usage
if __name__ == "__main__":
    queue = Queue()
    
    # Enqueue elements
    queue.enqueue(1)
    queue.enqueue(2)
    queue.enqueue(3)
    print(f"Queue: {queue}")  # Front -> [1, 2, 3] <- Rear
    
    # Front and Rear
    print(f"Front: {queue.front()}")  # 1
    print(f"Rear: {queue.rear()}")    # 3
    
    # Dequeue
    print(f"Dequeued: {queue.dequeue()}")  # 1
    print(f"Queue after dequeue: {queue}")  # Front -> [2, 3] <- Rear
    
    # Size
    print(f"Size: {queue.size()}")  # 2
    
    # isEmpty
    print(f"Is empty: {queue.is_empty()}")  # False
```

## Queue Using collections.deque (Efficient)

```python
from collections import deque

class EfficientQueue:
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, item):
        """Add item to rear - O(1)"""
        self.items.append(item)
    
    def dequeue(self):
        """Remove and return front item - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items.popleft()
    
    def front(self):
        """Return front item - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.items[0]
    
    def is_empty(self):
        """Check if empty - O(1)"""
        return len(self.items) == 0
    
    def size(self):
        """Get size - O(1)"""
        return len(self.items)
```

## Queue Using Linked List

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class QueueLinkedList:
    def __init__(self):
        self.front = None
        self.rear = None
        self._size = 0
    
    def enqueue(self, data):
        """Add element to rear - O(1)"""
        new_node = Node(data)
        
        if self.rear is None:
            self.front = self.rear = new_node
        else:
            self.rear.next = new_node
            self.rear = new_node
        
        self._size += 1
    
    def dequeue(self):
        """Remove and return front element - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        dequeued = self.front.data
        self.front = self.front.next
        
        if self.front is None:
            self.rear = None
        
        self._size -= 1
        return dequeued
    
    def get_front(self):
        """Return front element - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.front.data
    
    def is_empty(self):
        """Check if empty - O(1)"""
        return self.front is None
    
    def size(self):
        """Get size - O(1)"""
        return self._size
```

## Circular Queue

```python
class CircularQueue:
    def __init__(self, capacity):
        self.capacity = capacity
        self.queue = [None] * capacity
        self.front = -1
        self.rear = -1
        self._size = 0
    
    def enqueue(self, item):
        """Add item to rear - O(1)"""
        if self.is_full():
            raise OverflowError("Queue is full")
        
        if self.front == -1:
            self.front = 0
        
        self.rear = (self.rear + 1) % self.capacity
        self.queue[self.rear] = item
        self._size += 1
    
    def dequeue(self):
        """Remove and return front item - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        dequeued = self.queue[self.front]
        
        if self.front == self.rear:
            self.front = self.rear = -1
        else:
            self.front = (self.front + 1) % self.capacity
        
        self._size -= 1
        return dequeued
    
    def is_empty(self):
        """Check if empty - O(1)"""
        return self.front == -1
    
    def is_full(self):
        """Check if full - O(1)"""
        return (self.rear + 1) % self.capacity == self.front and self._size == self.capacity
    
    def size(self):
        """Get size - O(1)"""
        return self._size
```

## Priority Queue

```python
import heapq

class PriorityQueue:
    def __init__(self):
        self.heap = []
        self.counter = 0  # To handle same priority
    
    def enqueue(self, item, priority):
        """Add item with priority - O(log n)"""
        # Negative priority for max heap behavior
        heapq.heappush(self.heap, (priority, self.counter, item))
        self.counter += 1
    
    def dequeue(self):
        """Remove and return highest priority item - O(log n)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return heapq.heappop(self.heap)[2]
    
    def is_empty(self):
        """Check if empty - O(1)"""
        return len(self.heap) == 0
    
    def size(self):
        """Get size - O(1)"""
        return len(self.heap)
```

## Common Queue Problems

### 1. Implement Stack using Queues
```python
from collections import deque

class StackUsingQueues:
    def __init__(self):
        self.q1 = deque()
        self.q2 = deque()
    
    def push(self, x):
        """Push to stack - O(n)"""
        self.q2.append(x)
        
        while self.q1:
            self.q2.append(self.q1.popleft())
        
        self.q1, self.q2 = self.q2, self.q1
    
    def pop(self):
        """Pop from stack - O(1)"""
        if not self.q1:
            raise IndexError("Stack is empty")
        return self.q1.popleft()
```

### 2. Generate Binary Numbers
```python
def generate_binary_numbers(n):
    """Generate binary numbers from 1 to n"""
    queue = Queue()
    queue.enqueue("1")
    result = []
    
    for _ in range(n):
        front = queue.dequeue()
        result.append(front)
        
        queue.enqueue(front + "0")
        queue.enqueue(front + "1")
    
    return result

# Test
print(generate_binary_numbers(5))  # ['1', '10', '11', '100', '101']
```

## Example Implementation (JavaScript)

```javascript
class Queue {
    constructor() {
        this.items = [];
    }
    
    enqueue(element) {
        this.items.push(element);
    }
    
    dequeue() {
        if (this.isEmpty()) {
            throw new Error("Queue is empty");
        }
        return this.items.shift();
    }
    
    front() {
        if (this.isEmpty()) {
            throw new Error("Queue is empty");
        }
        return this.items[0];
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
}

// Example usage
const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
console.log(queue.front());    // 1
console.log(queue.dequeue());  // 1
console.log(queue.size());     // 2
```

## Practice Problems

1. **Implement Queue using Stacks**: Use two stacks to implement queue
2. **Sliding Window Maximum**: Find maximum in each sliding window
3. **First Non-Repeating Character**: Find first unique character in stream
4. **Reverse First K Elements**: Reverse first k elements of queue
5. **Interleave Queue**: Interleave first and second half

## Practice Resources

- LeetCode Queue Problems
- HackerRank Queues
- GeeksforGeeks Queue Data Structure

## Next Steps

After mastering queues, explore:
- [Hash Tables](../../intermediate/hash-tables/) - Efficient key-value storage
- [Trees](../../intermediate/trees/) - BFS uses queues
- Priority Queues and Heaps
