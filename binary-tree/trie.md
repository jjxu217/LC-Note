# Trie

## 211. Add and Search Word - Data structure design

Design a data structure that supports the following two operations:

```text
void addWord(word)
bool search(word)
```

search\(word\) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.

**Example:**

```text
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

### Sol: use dict to represent trie, key is child 

```python
class WordDictionary:
    def __init__(self):
        self.trie = {}

    def addWord(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None

    def search(self, word: str) -> bool:
        def find(word, node):
            if not word:
                return None in node         
            char, word = word[0], word[1:]
            if char == '.':
                return any(find(word, kid) for kid in node.values() if kid)     
            return char in node and find(word, node[char])
            
        return find(word, self.trie)
```

### sol: use TrieNode

```python
class TrieNode():
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.isWord = False
    
class WordDictionary(object):
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word):
        node = self.root
        for w in word:
            node = node.children[w]
        node.isWord = True

    def search(self, word):
        def find(word, node):
            if not word:
                return node.isWord
            char, word = word[0], word[1:]
            if char == '.':
                return any(find(word, kid) for kid in node.children.values())
            return char in node.children.keys() and find(word, node.children[char])
    
        return find(word, self.root)
```

## 208. Implement Trie \(Prefix Tree\)

Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```text
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

```python
class Trie:
    def __init__(self):
        self.trie = {}       

    def insert(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None      

    def search(self, word: str) -> bool:
        def find(word, node):
            if not word:
                return None in node         
            char, word = word[0], word[1:]
            return char in node and find(word, node[char])
            
        return find(word, self.trie)

    def startsWith(self, prefix: str) -> bool:
        def helper(prefix, node):
            if not prefix: 
                return True
            char, prefix = prefix[0], prefix[1:]
            return char in node and helper(prefix, node[char])
        
        return helper(prefix, self.trie)
```

