# Hash Tables

## Overview

A hash table (also known as hash map) is a data structure that implements an associative array abstract data type, mapping keys to values. It uses a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

## Characteristics

- **Key-Value Pairs**: Stores data as key-value associations
- **Fast Lookups**: Average O(1) time for search, insert, delete
- **Hash Function**: Converts keys to array indices
- **Collision Handling**: Manages multiple keys mapping to same index

## Time Complexity

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search    | O(1)    | O(n)       |
| Insert    | O(1)    | O(n)       |
| Delete    | O(1)    | O(n)       |

**Note**: Worst case occurs when all keys hash to the same index.

## Space Complexity

O(n) where n is the number of key-value pairs

## Hash Function

A good hash function should:
1. **Uniform Distribution**: Distribute keys evenly across the table
2. **Deterministic**: Same key always produces same hash
3. **Efficient**: Fast to compute
4. **Minimize Collisions**: Reduce keys mapping to same index

### Common Hash Functions

```python
# Simple modulo hash
def hash_mod(key, table_size):
    return key % table_size

# String hashing (polynomial rolling hash)
def hash_string(s, table_size):
    hash_value = 0
    prime = 31
    for char in s:
        hash_value = (hash_value * prime + ord(char)) % table_size
    return hash_value
```

## Collision Resolution

### 1. Chaining (Separate Chaining)
- Each bucket contains a linked list of entries
- Multiple keys with same hash stored in the list
- Simple to implement
- Performance degrades if many collisions

### 2. Open Addressing
- All entries stored in the table itself
- When collision occurs, probe for next available slot
- Probing methods:
  - **Linear Probing**: Check next slot (i+1, i+2, ...)
  - **Quadratic Probing**: Check slots at quadratic intervals (i+1², i+2², ...)
  - **Double Hashing**: Use second hash function

## Load Factor

Load factor = n / m (n = number of entries, m = table size)

- High load factor → more collisions
- Typically rehash when load factor > 0.7-0.75
- Rehashing: Create larger table and reinsert all entries

## Advantages

- **Fast Operations**: O(1) average time for basic operations
- **Flexible Keys**: Can use various data types as keys
- **Cache Efficient**: Good locality of reference
- **Easy Implementation**: Straightforward to code

## Disadvantages

- **Worst Case Performance**: O(n) when many collisions
- **Space Overhead**: May waste space for better performance
- **No Ordering**: Elements not stored in any particular order
- **Hash Function Dependency**: Performance depends on hash quality

## Use Cases

1. **Database Indexing**: Fast record lookup
2. **Caching**: Store frequently accessed data
3. **Counting Frequencies**: Count occurrences of elements
4. **Symbol Tables**: Compiler/interpreter symbol management
5. **Associative Arrays**: Implement dictionaries/maps
6. **Set Implementation**: Fast membership testing

## Example Implementation (Python)

```python
# Hash Table with Chaining

class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]
        self.count = 0
    
    def _hash(self, key):
        """Hash function using built-in hash"""
        return hash(key) % self.size
    
    def insert(self, key, value):
        """Insert key-value pair - O(1) average"""
        index = self._hash(key)
        
        # Check if key exists and update
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index][i] = (key, value)
                return
        
        # Key doesn't exist, add new entry
        self.table[index].append((key, value))
        self.count += 1
        
        # Rehash if load factor too high
        if self.count / self.size > 0.7:
            self._rehash()
    
    def get(self, key):
        """Get value for key - O(1) average"""
        index = self._hash(key)
        
        for k, v in self.table[index]:
            if k == key:
                return v
        
        raise KeyError(f"Key '{key}' not found")
    
    def delete(self, key):
        """Delete key-value pair - O(1) average"""
        index = self._hash(key)
        
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index].pop(i)
                self.count -= 1
                return v
        
        raise KeyError(f"Key '{key}' not found")
    
    def contains(self, key):
        """Check if key exists - O(1) average"""
        try:
            self.get(key)
            return True
        except KeyError:
            return False
    
    def _rehash(self):
        """Resize and rehash all entries"""
        old_table = self.table
        self.size *= 2
        self.table = [[] for _ in range(self.size)]
        self.count = 0
        
        for bucket in old_table:
            for key, value in bucket:
                self.insert(key, value)
    
    def keys(self):
        """Get all keys"""
        result = []
        for bucket in self.table:
            for key, _ in bucket:
                result.append(key)
        return result
    
    def values(self):
        """Get all values"""
        result = []
        for bucket in self.table:
            for _, value in bucket:
                result.append(value)
        return result
    
    def items(self):
        """Get all key-value pairs"""
        result = []
        for bucket in self.table:
            result.extend(bucket)
        return result
    
    def __len__(self):
        """Get number of entries"""
        return self.count
    
    def __str__(self):
        """String representation"""
        return str(dict(self.items()))

# Example usage
if __name__ == "__main__":
    ht = HashTable()
    
    # Insert
    ht.insert("name", "John")
    ht.insert("age", 30)
    ht.insert("city", "New York")
    print(f"Hash Table: {ht}")
    
    # Get
    print(f"Name: {ht.get('name')}")
    print(f"Age: {ht.get('age')}")
    
    # Update
    ht.insert("age", 31)
    print(f"Updated age: {ht.get('age')}")
    
    # Contains
    print(f"Contains 'name': {ht.contains('name')}")
    print(f"Contains 'country': {ht.contains('country')}")
    
    # Delete
    ht.delete("city")
    print(f"After deleting 'city': {ht}")
    
    # Keys, values, items
    print(f"Keys: {ht.keys()}")
    print(f"Values: {ht.values()}")
    print(f"Items: {ht.items()}")
```

## Hash Table with Open Addressing

```python
class HashTableOpenAddressing:
    def __init__(self, size=10):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size
        self.count = 0
    
    def _hash(self, key):
        """Hash function"""
        return hash(key) % self.size
    
    def _probe(self, index):
        """Linear probing"""
        return (index + 1) % self.size
    
    def insert(self, key, value):
        """Insert with linear probing - O(1) average"""
        if self.count / self.size > 0.7:
            self._rehash()
        
        index = self._hash(key)
        
        while self.keys[index] is not None:
            if self.keys[index] == key:
                self.values[index] = value
                return
            index = self._probe(index)
        
        self.keys[index] = key
        self.values[index] = value
        self.count += 1
    
    def get(self, key):
        """Get value - O(1) average"""
        index = self._hash(key)
        
        while self.keys[index] is not None:
            if self.keys[index] == key:
                return self.values[index]
            index = self._probe(index)
        
        raise KeyError(f"Key '{key}' not found")
    
    def _rehash(self):
        """Resize and rehash"""
        old_keys = self.keys
        old_values = self.values
        
        self.size *= 2
        self.keys = [None] * self.size
        self.values = [None] * self.size
        self.count = 0
        
        for i in range(len(old_keys)):
            if old_keys[i] is not None:
                self.insert(old_keys[i], old_values[i])
```

## Common Hash Table Problems

### 1. Two Sum
```python
def two_sum(nums, target):
    """Find two numbers that sum to target"""
    hash_map = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in hash_map:
            return [hash_map[complement], i]
        hash_map[num] = i
    
    return None

# Test
print(two_sum([2, 7, 11, 15], 9))  # [0, 1]
```

### 2. First Non-Repeating Character
```python
def first_non_repeating(s):
    """Find first non-repeating character"""
    char_count = {}
    
    # Count frequencies
    for char in s:
        char_count[char] = char_count.get(char, 0) + 1
    
    # Find first with count 1
    for char in s:
        if char_count[char] == 1:
            return char
    
    return None

# Test
print(first_non_repeating("leetcode"))  # 'l'
```

### 3. Group Anagrams
```python
def group_anagrams(strs):
    """Group anagrams together"""
    anagrams = {}
    
    for s in strs:
        key = ''.join(sorted(s))
        if key not in anagrams:
            anagrams[key] = []
        anagrams[key].append(s)
    
    return list(anagrams.values())

# Test
print(group_anagrams(["eat", "tea", "tan", "ate", "nat", "bat"]))
# [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
```

## Built-in Hash Tables

### Python Dictionary
```python
# Python's dict is a hash table
d = {"name": "John", "age": 30}
d["city"] = "New York"
print(d.get("name"))
print("age" in d)
```

### JavaScript Object/Map
```javascript
// JavaScript Object
const obj = {name: "John", age: 30};
obj.city = "New York";

// JavaScript Map
const map = new Map();
map.set("name", "John");
map.set("age", 30);
console.log(map.get("name"));
console.log(map.has("age"));
```

## Practice Problems

1. **Valid Anagram**: Check if two strings are anagrams
2. **Subarray Sum Equals K**: Find subarrays with given sum
3. **Longest Substring Without Repeating**: Find longest unique substring
4. **LRU Cache**: Implement Least Recently Used cache
5. **Design HashMap**: Implement your own hash map

## Practice Resources

- LeetCode Hash Table Problems
- HackerRank Hash Tables
- GeeksforGeeks Hashing

## Next Steps

- [Trees](../trees/) - Hierarchical data structure
- [Heaps](../heaps/) - Priority-based operations
- Advanced hashing techniques and applications
