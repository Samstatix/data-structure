# Stacks

## Overview

A stack is a linear data structure that follows the **LIFO (Last In First Out)** principle. The last element added to the stack will be the first one to be removed.

Think of it like a stack of plates - you add plates to the top and remove them from the top.

## Characteristics

- **LIFO Order**: Last In, First Out
- **Single End Operations**: All operations happen at one end (top)
- **Limited Access**: Can only access the top element
- **Dynamic Size**: Can grow and shrink as needed

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Push      | O(1)          |
| Pop       | O(1)          |
| Peek/Top  | O(1)          |
| isEmpty   | O(1)          |
| Search    | O(n)          |

## Space Complexity

O(n) where n is the number of elements

## Basic Operations

### 1. Push
Add an element to the top of the stack.

### 2. Pop
Remove and return the top element from the stack.

### 3. Peek/Top
Return the top element without removing it.

### 4. isEmpty
Check if the stack is empty.

### 5. Size
Get the number of elements in the stack.

## Implementation Methods

Stacks can be implemented using:
1. **Arrays**: Fixed or dynamic size
2. **Linked Lists**: Dynamic size, no overflow

## Advantages

- **Simple Operations**: Easy to implement and use
- **Fast Operations**: All operations are O(1)
- **Memory Management**: Efficient for function call management
- **Backtracking**: Natural fit for backtracking algorithms

## Disadvantages

- **Limited Access**: Can only access top element
- **Not Searchable**: Requires popping elements to search
- **Size Limitation**: Array-based implementation has fixed size

## Use Cases

1. **Function Call Stack**: Managing function calls and returns
2. **Undo/Redo Operations**: In text editors and applications
3. **Expression Evaluation**: Infix, postfix, prefix expressions
4. **Backtracking**: Maze solving, N-Queens problem
5. **Browser History**: Back button functionality
6. **Syntax Parsing**: Checking balanced parentheses
7. **Depth-First Search (DFS)**: Graph traversal algorithm

## Example Implementation (Python)

```python
# Stack implementation using list

class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        """Add an item to the top - O(1)"""
        self.items.append(item)
    
    def pop(self):
        """Remove and return the top item - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.items.pop()
    
    def peek(self):
        """Return the top item without removing - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.items[-1]
    
    def is_empty(self):
        """Check if stack is empty - O(1)"""
        return len(self.items) == 0
    
    def size(self):
        """Get the size of the stack - O(1)"""
        return len(self.items)
    
    def __str__(self):
        """String representation"""
        return str(self.items)

# Example usage
if __name__ == "__main__":
    stack = Stack()
    
    # Push elements
    stack.push(1)
    stack.push(2)
    stack.push(3)
    print(f"Stack after pushes: {stack}")  # [1, 2, 3]
    
    # Peek
    print(f"Top element: {stack.peek()}")  # 3
    
    # Pop
    print(f"Popped: {stack.pop()}")  # 3
    print(f"Stack after pop: {stack}")  # [1, 2]
    
    # Size
    print(f"Stack size: {stack.size()}")  # 2
    
    # isEmpty
    print(f"Is empty: {stack.is_empty()}")  # False
```

## Stack Using Linked List

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class StackLinkedList:
    def __init__(self):
        self.head = None
        self._size = 0
    
    def push(self, data):
        """Add element to top - O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
        self._size += 1
    
    def pop(self):
        """Remove and return top element - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        
        popped = self.head.data
        self.head = self.head.next
        self._size -= 1
        return popped
    
    def peek(self):
        """Return top element - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.head.data
    
    def is_empty(self):
        """Check if empty - O(1)"""
        return self.head is None
    
    def size(self):
        """Get size - O(1)"""
        return self._size
```

## Common Stack Problems

### 1. Balanced Parentheses
```python
def is_balanced(expression):
    """Check if parentheses are balanced"""
    stack = Stack()
    opening = "({["
    closing = ")}]"
    matches = {"(": ")", "{": "}", "[": "]"}
    
    for char in expression:
        if char in opening:
            stack.push(char)
        elif char in closing:
            if stack.is_empty():
                return False
            if matches[stack.pop()] != char:
                return False
    
    return stack.is_empty()

# Test
print(is_balanced("({[]})"))  # True
print(is_balanced("({[})"))   # False
```

### 2. Reverse a String
```python
def reverse_string(s):
    """Reverse a string using stack"""
    stack = Stack()
    
    # Push all characters
    for char in s:
        stack.push(char)
    
    # Pop all characters
    reversed_str = ""
    while not stack.is_empty():
        reversed_str += stack.pop()
    
    return reversed_str

# Test
print(reverse_string("hello"))  # "olleh"
```

### 3. Postfix Expression Evaluation
```python
def evaluate_postfix(expression):
    """Evaluate postfix expression"""
    stack = Stack()
    
    for token in expression.split():
        if token.isdigit():
            stack.push(int(token))
        else:
            operand2 = stack.pop()
            operand1 = stack.pop()
            
            if token == '+':
                stack.push(operand1 + operand2)
            elif token == '-':
                stack.push(operand1 - operand2)
            elif token == '*':
                stack.push(operand1 * operand2)
            elif token == '/':
                stack.push(operand1 // operand2)
    
    return stack.pop()

# Test
print(evaluate_postfix("2 3 + 4 *"))  # 20
```

### 4. Next Greater Element
```python
def next_greater_element(arr):
    """Find next greater element for each element"""
    stack = Stack()
    result = [-1] * len(arr)
    
    for i in range(len(arr) - 1, -1, -1):
        while not stack.is_empty() and stack.peek() <= arr[i]:
            stack.pop()
        
        if not stack.is_empty():
            result[i] = stack.peek()
        
        stack.push(arr[i])
    
    return result

# Test
print(next_greater_element([4, 5, 2, 10]))  # [5, 10, 10, -1]
```

## Example Implementation (JavaScript)

```javascript
class Stack {
    constructor() {
        this.items = [];
    }
    
    push(element) {
        this.items.push(element);
    }
    
    pop() {
        if (this.isEmpty()) {
            throw new Error("Stack is empty");
        }
        return this.items.pop();
    }
    
    peek() {
        if (this.isEmpty()) {
            throw new Error("Stack is empty");
        }
        return this.items[this.items.length - 1];
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
    
    clear() {
        this.items = [];
    }
}

// Example usage
const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
console.log(stack.peek());  // 3
console.log(stack.pop());   // 3
console.log(stack.size());  // 2
```

## Practice Problems

1. **Min Stack**: Design a stack that supports push, pop, and retrieving minimum element in O(1)
2. **Valid Parentheses**: Check if string has valid parentheses
3. **Daily Temperatures**: Find how many days until warmer temperature
4. **Largest Rectangle in Histogram**: Find largest rectangular area
5. **Implement Queue using Stacks**: Use two stacks to implement queue

## Practice Resources

- LeetCode Stack Problems
- HackerRank Stacks
- GeeksforGeeks Stack Data Structure

## Next Steps

After mastering stacks, explore:
- [Queues](../queues/) - FIFO data structure
- [Trees](../../intermediate/trees/) - Uses stack for DFS
- Expression parsing and evaluation algorithms
