# Google: 1032. Stream of Characters

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

