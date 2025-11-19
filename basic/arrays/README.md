# Arrays

## Overview

An array is a fundamental data structure that stores elements in contiguous memory locations. It provides constant-time access to elements by index.

## Characteristics

- **Fixed Size**: Most arrays have a fixed size (dynamic arrays can resize)
- **Contiguous Memory**: Elements are stored in consecutive memory locations
- **Index-Based Access**: Elements accessed using indices (0-based in most languages)
- **Homogeneous**: All elements are of the same data type

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Access    | O(1)          |
| Search    | O(n)          |
| Insertion (at end) | O(1) amortized |
| Insertion (at beginning/middle) | O(n) |
| Deletion (from end) | O(1) |
| Deletion (from beginning/middle) | O(n) |

## Space Complexity

O(n) where n is the number of elements

## Basic Operations

### 1. Traversal
Visiting each element in the array sequentially.

### 2. Insertion
Adding a new element to the array.

### 3. Deletion
Removing an element from the array.

### 4. Search
Finding an element in the array.

## Advantages

- **Fast Access**: O(1) time to access any element by index
- **Cache Friendly**: Contiguous memory improves cache performance
- **Simple**: Easy to understand and implement

## Disadvantages

- **Fixed Size**: Cannot easily change size (for static arrays)
- **Insertion/Deletion**: Expensive operations for large arrays
- **Memory Waste**: May allocate more memory than needed

## Use Cases

1. **Storing Sequential Data**: When you need to store a collection of items
2. **Lookup Tables**: Fast access to data by index
3. **Implementation of Other Structures**: Base for stacks, queues, heaps
4. **Matrix Operations**: 2D arrays for mathematical computations

## Example Implementation (Python)

```python
# Array operations in Python (using lists)

# 1. Creation
arr = [1, 2, 3, 4, 5]
print(f"Array: {arr}")

# 2. Access
print(f"Element at index 2: {arr[2]}")  # O(1)

# 3. Insertion
arr.append(6)  # O(1) at end
print(f"After append: {arr}")

arr.insert(0, 0)  # O(n) at beginning
print(f"After insert at beginning: {arr}")

# 4. Deletion
arr.pop()  # O(1) from end
print(f"After pop: {arr}")

arr.pop(0)  # O(n) from beginning
print(f"After pop from beginning: {arr}")

# 5. Search
if 3 in arr:  # O(n)
    index = arr.index(3)
    print(f"Found 3 at index: {index}")

# 6. Traversal
print("Traversing array:")
for i, element in enumerate(arr):
    print(f"Index {i}: {element}")

# 7. Update
arr[2] = 10  # O(1)
print(f"After update: {arr}")

# 8. Length
print(f"Array length: {len(arr)}")
```

## Example Implementation (JavaScript)

```javascript
// Array operations in JavaScript

// 1. Creation
let arr = [1, 2, 3, 4, 5];
console.log("Array:", arr);

// 2. Access
console.log("Element at index 2:", arr[2]);  // O(1)

// 3. Insertion
arr.push(6);  // O(1) at end
console.log("After push:", arr);

arr.unshift(0);  // O(n) at beginning
console.log("After unshift:", arr);

// 4. Deletion
arr.pop();  // O(1) from end
console.log("After pop:", arr);

arr.shift();  // O(n) from beginning
console.log("After shift:", arr);

// 5. Search
const index = arr.indexOf(3);  // O(n)
if (index !== -1) {
    console.log("Found 3 at index:", index);
}

// 6. Traversal
console.log("Traversing array:");
arr.forEach((element, i) => {
    console.log(`Index ${i}: ${element}`);
});

// 7. Update
arr[2] = 10;  // O(1)
console.log("After update:", arr);

// 8. Length
console.log("Array length:", arr.length);
```

## Common Array Problems

1. **Two Sum**: Find two numbers that add up to a target
2. **Maximum Subarray**: Find contiguous subarray with maximum sum (Kadane's Algorithm)
3. **Rotate Array**: Rotate array elements by k positions
4. **Remove Duplicates**: Remove duplicate elements from sorted array
5. **Merge Sorted Arrays**: Merge two sorted arrays into one

## Related Concepts

- **Dynamic Arrays**: Arrays that can resize (ArrayList in Java, vector in C++)
- **Multi-dimensional Arrays**: Arrays of arrays (matrices)
- **Circular Arrays**: Last element connects to first element

## Practice Resources

- LeetCode Array Problems
- HackerRank Arrays
- GeeksforGeeks Array Data Structure

## Next Steps

After mastering arrays, move on to:
- [Linked Lists](../linked-lists/) - Dynamic alternative to arrays
- [Stacks](../stacks/) - Can be implemented using arrays
- [Queues](../queues/) - Can be implemented using arrays
