# Bloom Filters

## Overview

A Bloom filter is a space-efficient probabilistic data structure used to test whether an element is a member of a set. It can definitively say "definitely not in set" or "possibly in set" but never gives false negatives.

## Characteristics

- **Probabilistic**: Can have false positives, but no false negatives
- **Space Efficient**: Much smaller than storing all elements
- **Fixed Size**: Size determined at creation
- **No Deletion**: Standard Bloom filters don't support deletion
- **Fast Operations**: O(k) where k is number of hash functions

## Key Properties

- If Bloom filter says "NO" → element is **definitely not** in set
- If Bloom filter says "YES" → element is **possibly** in set
- False positive rate depends on:
  - Size of bit array (m)
  - Number of hash functions (k)
  - Number of inserted elements (n)

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Insert    | O(k) |
| Query     | O(k) |

Where k is the number of hash functions (typically small constant like 3-10).

## Space Complexity

O(m) where m is the size of bit array (much smaller than O(n) for storing actual elements).

## Mathematical Foundation

**Optimal number of hash functions:**
```
k = (m/n) * ln(2)
```

**False positive probability:**
```
p ≈ (1 - e^(-kn/m))^k
```

Where:
- m = size of bit array
- n = number of elements inserted
- k = number of hash functions
- p = false positive rate

## Implementation

```python
import hashlib

class BloomFilter:
    def __init__(self, size, num_hash_functions):
        """
        Initialize Bloom filter
        
        Args:
            size: Size of bit array
            num_hash_functions: Number of hash functions to use
        """
        self.size = size
        self.num_hash = num_hash_functions
        self.bit_array = [False] * size
        self.count = 0
    
    def _hash(self, item, seed):
        """Generate hash value using seed"""
        h = hashlib.md5((str(item) + str(seed)).encode())
        return int(h.hexdigest(), 16) % self.size
    
    def add(self, item):
        """Add item to Bloom filter - O(k)"""
        for i in range(self.num_hash):
            index = self._hash(item, i)
            self.bit_array[index] = True
        self.count += 1
    
    def contains(self, item):
        """Check if item might be in set - O(k)"""
        for i in range(self.num_hash):
            index = self._hash(item, i)
            if not self.bit_array[index]:
                return False  # Definitely not in set
        return True  # Possibly in set
    
    def false_positive_rate(self):
        """Estimate current false positive rate"""
        import math
        k = self.num_hash
        n = self.count
        m = self.size
        
        if n == 0:
            return 0
        
        return (1 - math.exp(-k * n / m)) ** k
    
    def __len__(self):
        """Return number of items added (approximate)"""
        return self.count

# Example usage
if __name__ == "__main__":
    # Create Bloom filter
    bf = BloomFilter(size=1000, num_hash_functions=3)
    
    # Add elements
    words = ["apple", "banana", "cherry", "date"]
    for word in words:
        bf.add(word)
    
    # Check membership
    print(f"'apple' in filter: {bf.contains('apple')}")      # True (correct)
    print(f"'banana' in filter: {bf.contains('banana')}")    # True (correct)
    print(f"'grape' in filter: {bf.contains('grape')}")      # False or True (might be false positive)
    print(f"'xyz' in filter: {bf.contains('xyz')}")          # False (definitely not in set)
    
    # False positive rate
    print(f"False positive rate: {bf.false_positive_rate():.4f}")
```

## Optimal Bloom Filter

```python
import math
import hashlib

class OptimalBloomFilter:
    def __init__(self, expected_items, false_positive_rate=0.01):
        """
        Create optimally sized Bloom filter
        
        Args:
            expected_items: Expected number of items to insert
            false_positive_rate: Desired false positive rate (default 1%)
        """
        self.n = expected_items
        self.p = false_positive_rate
        
        # Calculate optimal size
        self.size = self._optimal_size(expected_items, false_positive_rate)
        
        # Calculate optimal number of hash functions
        self.num_hash = self._optimal_hash_count(self.size, expected_items)
        
        self.bit_array = [False] * self.size
        self.count = 0
    
    def _optimal_size(self, n, p):
        """Calculate optimal bit array size"""
        m = -(n * math.log(p)) / (math.log(2) ** 2)
        return int(m)
    
    def _optimal_hash_count(self, m, n):
        """Calculate optimal number of hash functions"""
        k = (m / n) * math.log(2)
        return max(1, int(k))
    
    def _hash(self, item, seed):
        """Generate hash value"""
        h = hashlib.md5((str(item) + str(seed)).encode())
        return int(h.hexdigest(), 16) % self.size
    
    def add(self, item):
        """Add item to filter"""
        for i in range(self.num_hash):
            index = self._hash(item, i)
            self.bit_array[index] = True
        self.count += 1
    
    def contains(self, item):
        """Check if item possibly in set"""
        for i in range(self.num_hash):
            index = self._hash(item, i)
            if not self.bit_array[index]:
                return False
        return True
    
    def info(self):
        """Print filter statistics"""
        print(f"Expected items: {self.n}")
        print(f"Bit array size: {self.size}")
        print(f"Number of hash functions: {self.num_hash}")
        print(f"Items added: {self.count}")
        print(f"Target false positive rate: {self.p:.4f}")
        
        bits_set = sum(self.bit_array)
        print(f"Bits set: {bits_set}/{self.size} ({bits_set/self.size*100:.2f}%)")

# Example
bf = OptimalBloomFilter(expected_items=1000, false_positive_rate=0.01)
bf.info()
```

## Counting Bloom Filter

Supports deletion by using counters instead of bits.

```python
class CountingBloomFilter:
    def __init__(self, size, num_hash_functions):
        self.size = size
        self.num_hash = num_hash_functions
        self.counters = [0] * size
    
    def _hash(self, item, seed):
        h = hashlib.md5((str(item) + str(seed)).encode())
        return int(h.hexdigest(), 16) % self.size
    
    def add(self, item):
        """Add item and increment counters"""
        for i in range(self.num_hash):
            index = self._hash(item, i)
            self.counters[index] += 1
    
    def remove(self, item):
        """Remove item and decrement counters"""
        if not self.contains(item):
            return False
        
        for i in range(self.num_hash):
            index = self._hash(item, i)
            if self.counters[index] > 0:
                self.counters[index] -= 1
        return True
    
    def contains(self, item):
        """Check if item possibly in set"""
        for i in range(self.num_hash):
            index = self._hash(item, i)
            if self.counters[index] == 0:
                return False
        return True
```

## Use Cases

### 1. Cache Filtering
Quickly check if item is in cache before expensive lookup.

```python
# Pseudocode
bloom_filter = BloomFilter(...)
cache = {}

def get_data(key):
    if not bloom_filter.contains(key):
        return fetch_from_database(key)
    
    if key in cache:
        return cache[key]
    
    data = fetch_from_database(key)
    cache[key] = data
    bloom_filter.add(key)
    return data
```

### 2. Spell Checker
Quick check if word is in dictionary.

### 3. Malicious URL Detection
Check if URL is in blacklist.

### 4. Database Query Optimization
Check if value exists before expensive disk read.

### 5. Network Routers
Check routing tables efficiently.

### 6. Cryptocurrency
Bitcoin uses Bloom filters for SPV (Simplified Payment Verification).

## Advantages

- **Space Efficient**: Uses much less space than hash table
- **Fast Operations**: O(k) insert and query
- **Fixed Memory**: Memory usage doesn't grow with items
- **No False Negatives**: Never misses an element
- **Parallel Operations**: Easy to implement in parallel

## Disadvantages

- **False Positives**: Can incorrectly report element exists
- **No Deletion**: Standard version doesn't support removal
- **Fixed Size**: Must know expected size in advance
- **No Element Retrieval**: Cannot retrieve stored elements
- **Increasing False Positive Rate**: Gets worse as more items added

## Variants

### 1. Counting Bloom Filter
- Uses counters instead of bits
- Supports deletion
- Uses more space

### 2. Scalable Bloom Filter
- Grows dynamically
- Multiple Bloom filters with different sizes
- Maintains target false positive rate

### 3. Cuckoo Filter
- Alternative to Bloom filter
- Supports deletion
- Better space efficiency
- Slightly slower operations

### 4. Quotient Filter
- Similar to Bloom filter
- Supports deletion
- Better cache performance

## Practical Example: Web Crawler

```python
class WebCrawler:
    def __init__(self):
        # Track visited URLs
        self.visited = OptimalBloomFilter(
            expected_items=1000000,
            false_positive_rate=0.001
        )
        self.url_queue = []
    
    def crawl(self, start_url):
        self.url_queue.append(start_url)
        
        while self.url_queue:
            url = self.url_queue.pop(0)
            
            # Skip if likely visited
            if self.visited.contains(url):
                continue
            
            # Mark as visited
            self.visited.add(url)
            
            # Process URL
            self.process_url(url)
            
            # Add new URLs to queue
            new_urls = self.extract_urls(url)
            self.url_queue.extend(new_urls)
    
    def process_url(self, url):
        # Process the URL
        pass
    
    def extract_urls(self, url):
        # Extract URLs from page
        return []
```

## Choosing Parameters

For a Bloom filter with:
- **n** = expected number of elements
- **p** = desired false positive rate

**Optimal bit array size:**
```python
m = -n * ln(p) / (ln(2))^2
```

**Optimal hash functions:**
```python
k = (m/n) * ln(2)
```

Example: For n=1,000,000 and p=0.01 (1%):
- m ≈ 9,585,059 bits ≈ 1.14 MB
- k ≈ 7 hash functions

## Practice Problems

1. **Design Bloom Filter**: Implement basic Bloom filter
2. **URL Deduplication**: Remove duplicate URLs in crawler
3. **Spell Checker**: Check if word is valid
4. **Rate Limiting**: Track requests within time window
5. **Duplicate Detection**: Find duplicates in stream

## Practice Resources

- System Design Interview Questions
- Distributed Systems implementations
- Database internals

## Next Steps

- Understand probabilistic data structures
- Study count-min sketch, HyperLogLog
- Learn about distributed Bloom filters
- Explore applications in big data systems
