# Tries (Prefix Trees)

## Overview

A trie, also called a prefix tree, is a tree-like data structure used to store and retrieve strings efficiently. Each node represents a character, and paths from root to nodes represent prefixes of stored strings.

## Characteristics

- **Character-Based Nodes**: Each node represents a character
- **Shared Prefixes**: Common prefixes share the same path
- **Efficient String Operations**: Fast prefix-based searches
- **Variable Depth**: Depth equals length of longest string

## Structure

- **Root**: Empty node (represents empty string)
- **Internal Nodes**: Characters in the middle of words
- **Leaf Markers**: Flag indicating end of a word
- **Children**: Map/array of child nodes (one per possible character)

## Time Complexity

| Operation | Time Complexity |
|-----------|----------------|
| Insert    | O(m) where m is key length |
| Search    | O(m) where m is key length |
| Delete    | O(m) where m is key length |
| Prefix Search | O(p) where p is prefix length |

## Space Complexity

O(ALPHABET_SIZE × N × M) where:
- ALPHABET_SIZE = number of possible characters
- N = number of keys
- M = average key length

Can be optimized to O(N × M) with dynamic allocation.

## Trie Node Implementation

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # Map of character to TrieNode
        self.is_end_of_word = False
        self.word_count = 0  # Optional: count of words with this prefix

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        """Insert a word into trie - O(m)"""
        node = self.root
        
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            node.word_count += 1
        
        node.is_end_of_word = True
    
    def search(self, word):
        """Search for exact word - O(m)"""
        node = self._find_node(word)
        return node is not None and node.is_end_of_word
    
    def starts_with(self, prefix):
        """Check if any word starts with prefix - O(p)"""
        return self._find_node(prefix) is not None
    
    def _find_node(self, prefix):
        """Helper to find node for given prefix"""
        node = self.root
        
        for char in prefix:
            if char not in node.children:
                return None
            node = node.children[char]
        
        return node
    
    def delete(self, word):
        """Delete a word from trie - O(m)"""
        def _delete_recursive(node, word, index):
            if index == len(word):
                # End of word reached
                if not node.is_end_of_word:
                    return False
                
                node.is_end_of_word = False
                node.word_count -= 1
                
                # Delete node if it has no children
                return len(node.children) == 0
            
            char = word[index]
            if char not in node.children:
                return False
            
            child = node.children[char]
            should_delete = _delete_recursive(child, word, index + 1)
            
            if should_delete:
                del node.children[char]
                node.word_count -= 1
                return len(node.children) == 0 and not node.is_end_of_word
            
            return False
        
        _delete_recursive(self.root, word, 0)
    
    def get_all_words(self):
        """Get all words in trie"""
        words = []
        
        def _dfs(node, prefix):
            if node.is_end_of_word:
                words.append(prefix)
            
            for char, child in node.children.items():
                _dfs(child, prefix + char)
        
        _dfs(self.root, "")
        return words
    
    def get_words_with_prefix(self, prefix):
        """Get all words with given prefix"""
        node = self._find_node(prefix)
        if not node:
            return []
        
        words = []
        
        def _dfs(node, current_prefix):
            if node.is_end_of_word:
                words.append(current_prefix)
            
            for char, child in node.children.items():
                _dfs(child, current_prefix + char)
        
        _dfs(node, prefix)
        return words
    
    def count_words_with_prefix(self, prefix):
        """Count words with given prefix"""
        node = self._find_node(prefix)
        return node.word_count if node else 0

# Example usage
if __name__ == "__main__":
    trie = Trie()
    
    # Insert words
    words = ["apple", "app", "apricot", "banana", "band"]
    for word in words:
        trie.insert(word)
    
    # Search
    print(f"Search 'apple': {trie.search('apple')}")  # True
    print(f"Search 'app': {trie.search('app')}")      # True
    print(f"Search 'appl': {trie.search('appl')}")    # False
    
    # Prefix search
    print(f"Starts with 'app': {trie.starts_with('app')}")  # True
    print(f"Starts with 'ban': {trie.starts_with('ban')}")  # True
    print(f"Starts with 'cat': {trie.starts_with('cat')}")  # False
    
    # Get words with prefix
    print(f"Words with prefix 'app': {trie.get_words_with_prefix('app')}")
    # ['apple', 'app', 'apricot']
    
    # Count
    print(f"Count with prefix 'ap': {trie.count_words_with_prefix('ap')}")
    
    # All words
    print(f"All words: {trie.get_all_words()}")
    
    # Delete
    trie.delete('app')
    print(f"After deleting 'app': {trie.search('app')}")  # False
    print(f"Apple still exists: {trie.search('apple')}")  # True
```

## Array-Based Trie (for lowercase letters)

```python
class ArrayTrieNode:
    def __init__(self):
        self.children = [None] * 26  # For 'a' to 'z'
        self.is_end_of_word = False

class ArrayTrie:
    def __init__(self):
        self.root = ArrayTrieNode()
    
    def _char_to_index(self, char):
        """Convert character to index (0-25)"""
        return ord(char) - ord('a')
    
    def insert(self, word):
        """Insert word - O(m)"""
        node = self.root
        
        for char in word.lower():
            index = self._char_to_index(char)
            if not node.children[index]:
                node.children[index] = ArrayTrieNode()
            node = node.children[index]
        
        node.is_end_of_word = True
    
    def search(self, word):
        """Search for word - O(m)"""
        node = self.root
        
        for char in word.lower():
            index = self._char_to_index(char)
            if not node.children[index]:
                return False
            node = node.children[index]
        
        return node.is_end_of_word
```

## Common Trie Problems

### 1. Autocomplete/Word Suggestions
```python
def autocomplete(trie, prefix, max_results=5):
    """Get autocomplete suggestions"""
    suggestions = trie.get_words_with_prefix(prefix)
    return suggestions[:max_results]
```

### 2. Longest Common Prefix
```python
def longest_common_prefix(words):
    """Find longest common prefix using trie"""
    if not words:
        return ""
    
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    lcp = ""
    node = trie.root
    
    while len(node.children) == 1 and not node.is_end_of_word:
        char = list(node.children.keys())[0]
        lcp += char
        node = node.children[char]
    
    return lcp
```

### 3. Word Search II (Board)
```python
def find_words(board, words):
    """Find all words from list that exist in board"""
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    result = set()
    rows, cols = len(board), len(board[0])
    
    def dfs(r, c, node, path):
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return
        
        char = board[r][c]
        if char not in node.children:
            return
        
        node = node.children[char]
        path += char
        
        if node.is_end_of_word:
            result.add(path)
        
        # Mark as visited
        temp = board[r][c]
        board[r][c] = '#'
        
        # Explore neighbors
        for dr, dc in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
            dfs(r + dr, c + dc, node, path)
        
        # Restore
        board[r][c] = temp
    
    for r in range(rows):
        for c in range(cols):
            dfs(r, c, trie.root, "")
    
    return list(result)
```

### 4. Add and Search Word (with wildcards)
```python
class WordDictionary:
    def __init__(self):
        self.root = TrieNode()
    
    def add_word(self, word):
        """Add word to dictionary"""
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        """Search with '.' as wildcard"""
        def dfs(node, i):
            if i == len(word):
                return node.is_end_of_word
            
            char = word[i]
            
            if char == '.':
                # Wildcard: try all children
                for child in node.children.values():
                    if dfs(child, i + 1):
                        return True
                return False
            else:
                if char not in node.children:
                    return False
                return dfs(node.children[char], i + 1)
        
        return dfs(self.root, 0)
```

### 5. Replace Words (Dictionary)
```python
def replace_words(dictionary, sentence):
    """Replace words with shortest root from dictionary"""
    trie = Trie()
    for root in dictionary:
        trie.insert(root)
    
    def find_root(word):
        node = trie.root
        prefix = ""
        
        for char in word:
            if char not in node.children:
                return word
            
            prefix += char
            node = node.children[char]
            
            if node.is_end_of_word:
                return prefix
        
        return word
    
    words = sentence.split()
    return " ".join(find_root(word) for word in words)
```

## Advanced Trie Variants

### Compressed Trie (Radix Tree)
- Merges nodes with single child
- More space efficient
- Used in routing tables, IP lookup

### Suffix Trie
- Stores all suffixes of a string
- Used for pattern matching
- Can find substring in O(m) time

### Ternary Search Trie (TST)
- Each node has 3 children (less, equal, greater)
- More space efficient than standard trie
- Good for large alphabets

## Use Cases

1. **Autocomplete**: Search suggestions, type-ahead
2. **Spell Checker**: Finding similar words
3. **IP Routing**: Longest prefix matching
4. **Dictionary**: Word validation and lookup
5. **DNA Sequence**: Pattern matching in genomics
6. **T9 Predictive Text**: Old phone text input
7. **File Systems**: Path lookup
8. **Search Engines**: Query suggestions

## Advantages

- **Fast Prefix Searches**: O(m) for prefix of length m
- **No Hash Collisions**: Unlike hash tables
- **Alphabetically Ordered**: Can traverse in order
- **Prefix Sharing**: Efficient storage of common prefixes

## Disadvantages

- **Space Intensive**: Can use a lot of memory
- **Slower Than Hash**: For exact match, hash table is faster
- **Cache Unfriendly**: Scattered memory access
- **Complex Implementation**: More complex than other structures

## Optimization Techniques

1. **Compressed Trie**: Merge single-child nodes
2. **Lazy Deletion**: Mark as deleted instead of removing
3. **Reference Counting**: Track word frequency
4. **Alphabet Reduction**: Use smaller character set
5. **Lazy Propagation**: Delay updates until needed

## Practice Problems

1. **Implement Trie**: Basic insert, search, startsWith
2. **Word Search II**: Find words in 2D board
3. **Design Add and Search**: With wildcard support
4. **Replace Words**: Dictionary-based replacement
5. **Maximum XOR**: Find maximum XOR of two numbers
6. **Stream of Characters**: Check if suffix forms word

## Practice Resources

- LeetCode Trie Problems
- HackerRank Trie
- GeeksforGeeks Trie Data Structure

## Next Steps

- [Advanced Trees](../advanced-trees/) - Segment trees, Fenwick trees
- [Union-Find](../union-find/) - Disjoint set operations
- Suffix arrays and suffix trees
