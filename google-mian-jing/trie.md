# Trie

## Random Stream

Stream: backiuwcatbeforewerehpqojf Input: \["back", "before", "cat", fore", "were", "for"\] Output: \[0, 10, 7, 12, 16, 12\] Generator: char getNextChar\(\);

给一个random stream，只能用getNextChar\(\);获取下一个char，给一个list of input words，找每个word在stream中出现的位置（出现位置） 假设stream够长，即使是random，也一定会出现每个word

My idea would be to build a trie from the input words. Keep a list of trie nodes as candidates, and for each new character read, try to extend each candidate node. If a word is reached by a node, we immediately know its length.

```python
class TrieNode():
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.pos = -1
        self.length = -1
        
class Trie():
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word, i, length):
        node = self.root
        for w in word:
            node = node.children[w]
        node.pos = i
        node.length = length
        
class StreamIndex:
    def __init__(self, words: List[str]):
        self.t = Trie()
        self.cand = [self.t.root]
        for i, word in enumerate(words):
            self.t.addWord(word, i, len(word))
            
        n = len(word)
        res = [0] * n
        cnt = idx = 0 #已经找到的word数，stream中的idx
        while cnt < n:
            char = getNextChar()      
            nxt_cand = [] #contain the trie node of next candidate
            for c in self.cand:
                if char in c.children:
                    node = c.children[char]
                    nxt_cand.append(node)
                    #如果找到一个词, 相应结果记录其实的index
                    if node.pos >= 0:
                        cnt += 1
                        res[node.pos] = idx - node.length + 1
                        node.pos = -1  
            idx += 1       
            self.cand = nxt_cand + [self.t.root]
        return res
```

## 1032. Stream of Characters

Implement the `StreamChecker` class as follows:

* `StreamChecker(words)`: Constructor, init the data structure with the given words.
* `query(letter)`: returns true if and only if for some `k >= 1`, the last `k` characters queried \(in order from oldest to newest, including this letter just queried\) spell one of the words in the given list.

**Example:**

```text
StreamChecker streamChecker = new StreamChecker(["cd","f","kl"]); // init the dictionary.
streamChecker.query('a');          // return false
streamChecker.query('b');          // return false
streamChecker.query('c');          // return false
streamChecker.query('d');          // return true, because 'cd' is in the wordlist
streamChecker.query('e');          // return false
streamChecker.query('f');          // return true, because 'f' is in the wordlist
streamChecker.query('g');          // return false
streamChecker.query('h');          // return false
streamChecker.query('i');          // return false
streamChecker.query('j');          // return false
streamChecker.query('k');          // return false
streamChecker.query('l');          // return true, because 'kl' is in the wordlist
```

### Trie\(\):

**Time Complexity**:  
`cand.size <= W`, where `W` is the maximum length of words.  
So that `O(query) = O(cand.size) = O(W)`  
We will make `Q` queries, the overall time complexity is `O(QW)`

```python
class Trie():
    def __init__(self):
        self.trie = {}

    def addWord(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None
        
class StreamChecker:
    def __init__(self, words: List[str]):
        self.t = Trie()
        self.cand = [self.t.trie]
        for word in words:
            self.t.addWord(word)

    def query(self, letter: str) -> bool:
        temp = []
        for c in self.cand:
            if letter in c:
                temp.append(c[letter])
                
        self.cand = temp + [self.t.trie]
        return any(None in i for i in temp)
```

