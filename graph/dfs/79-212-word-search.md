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
        if not board or not board[0] or not word: return False
        m, n = len(board), len(board[0])
        
        def dfs(x, y, word):    
            if board[x][y] == word[0]: #
                if len(word) == 1: #find the word
                    return True
                #checked cur pos
                seen.add((x, y))
                
                #check nei pos
                for X, Y in ((x+1,y),(x-1,y),(x,y-1), (x,y+1)):
                    if 0 <= X < m and 0 <= Y < n and (X, Y) not in seen:
                        if dfs(X, Y, word[1:]):
                            return True
                #backtrack
                seen.remove((x, y))
            return False
        
        for i in range(m):
            for j in range(n):
                seen = set() #deal with unique path
                if dfs(i, j, word):
                    return True
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

##  Similar to 980. Unique Paths III

{% page-ref page="../../untitled/62-63-64-path.md" %}



