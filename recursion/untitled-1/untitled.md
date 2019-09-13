# 51/52 N-Queens

## 51. N-Queens

![](https://assets.leetcode.com/uploads/2018/10/12/8-queens.png)

Given an integer _n_, return all distinct solutions to the _n_-queens puzzle.

**Example:**

```text
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

## 52. N-Queens II

Return the number of distinct solutions to the _n_-queens puzzle.

**Example:**

```text
Input: 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown below.
```

### Idea

![](../../.gitbook/assets/image%20%2810%29.png)

![](../../.gitbook/assets/image%20%2828%29.png)

```python
#51
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        self.res = []
        self.dfs([-1] * n, 0, n)
        return self.res
        
    def dfs(self, layout, cur, n):
        if cur == n:
            sol = []
            for i in range(n):
                sol.append("".join(['.'] * layout[i] + ['Q'] + ['.']*(n - layout[i] -1)))
            self.res.append(sol)
            return 
        for i in range(n):
            valid = True
            for j in range(cur):
                if layout[j] == i or cur - j == i - layout[j] or cur - j == layout[j] - i:
                    valid = False
                    break
            if valid:
                layout[cur] = i
                self.dfs(layout, cur + 1, n)
```

```python
#52
class Solution:
    def totalNQueens(self, n: int) -> int:
        self.res = 0
        self.dfs([-1] * n, 0, n)
        return self.res
        
    def dfs(self, layout, cur, n):
        if cur == n:
            self.res += 1
            return 
        for i in range(n):
            valid = True
            for j in range(cur):
                if layout[j] == i or cur - j == i - layout[j] or cur - j == layout[j] - i:
                    valid = False
                    break
            if valid:
                layout[cur] = i
                self.dfs(layout, cur + 1, n)
                
```

