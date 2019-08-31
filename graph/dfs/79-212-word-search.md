# 79/212 Word Search

## 79. Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```text
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

### DFS:

```python
class Solution:        
    def exist(self, board: List[List[str]], word: str) -> bool:
        if not board or not board[0]: return False
        if not word: return True
        
        m, n = len(board), len(board[0])      
        for i in range(m):
            for j in range(n):
                if board[i][j] == word[0]:
                    if self.dfs(board, i, j, word[1:]):
                        return True
        return False
    
    def dfs(self, board, i, j, word):   
        m, n = len(board), len(board[0])      
        if len(word) == 0: # all the characters are checked
            return True
        
        # checked cur
        tmp = board[i][j]  
        board[i][j] = "#"  # avoid visit agian 
        
        # check nei
        for a, b in ((i+1,j),(i-1,j),(i,j+1),(i,j-1)):
            if 0 <= a < m and 0 <= b < n and board[a][b] == word[0]:
                if self.dfs(board, a, b, word[1:]):
                    return True
        #backtrack
        board[i][j] = tmp
        return False
```

## 212. Word Search II

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example:**

```text
Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

```python
class Trie:
    def __init__(self):
        self.trie = {}

    def add(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None
        
class Solution:         
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        if not board or not board[0] or not words: return []
        m, n = len(board), len(board[0])
        self.res = []
        t = Trie()       
        for word in words:  #add words to tries
            t.add(word)
        
        for i in range(m):
            for j in range(n):
                self.dfs(board, i, j, '', t.trie)
        return self.res
        
    def dfs(self, board, x, y, path, node):     
        if board[x][y] in node:
            m, n = len(board), len(board[0])
            
            #check cur pos
            node = node[board[x][y]]
            path += str(board[x][y])
            if None in node: #find the word
                self.res.append(path)
                del node[None] 
            #avoid walk again
            temp = board[x][y]
            board[x][y] = '#' 
            #check nei pos
            for X, Y in ((x+1,y),(x-1,y),(x,y-1), (x,y+1)):
                if 0 <= X < m and 0 <= Y < n:
                    self.dfs(board, X, Y, path, node)
            #backtrack
            board[x][y] = temp
```

##  Similar to 980. Unique Paths III

{% page-ref page="../../untitled/62-63-64-path.md" %}



