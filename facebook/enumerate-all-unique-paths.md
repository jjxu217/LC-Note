# Enumerate All Unique Paths

Ran into this during my recent facebook onsite, it's similar to the [62. Unique Paths](https://leetcode.com/problems/unique-paths/) problem, but the guy wanted to enumerate all the paths \(\['DDRR', 'RDDR', 'DRDR'...\]\) and give the run-time of the solution, which he said was the main part of the question.

Instead of m \* n grid, he simple said the map is a n \* n square. I wrote the following backtracking code and give said the runtime is O\(2^n\). He said it's "good enough", but I'm pretty sure he was looking for a more specific answer. I've read up on the solutions of Unique Paths and realized the \# of paths are C\(m+n-2, m-1\) so is the accurate runtime O\(C\(2n-2, n-1\) \* n\) since each path is on the order of size n? I think this makes sense but cannot correlate it to my code. Anyways, please comment on my code here, and let me know if the runtime makes sense, or have u come up with a more specific runtime. Thanks!

```python
def uniquePaths(self, m: int, n: int) -> list:
    return list(itertools.permuatations('D'*n + 'R'*m))
    
    
import collections
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = collections.defaultdict(list)

        for i in range(m):
            for j in range(n):
                if i == 0 and j == 0:
                    dp[0, 0] = ['']
                elif i == 0:
                    dp[0, j] = [pre+'D' for pre in dp[0, j-1]]
                elif j == 0:
                    dp[i, 0] = [pre+'R' for pre in dp[i-1, 0]]
                else:
                    dp[i, j] = [pre+'R' for pre in dp[i-1, j]] \
                                + [pre+'D' for pre in dp[i, j-1]]
        return dp[m-1,n-1]
```

{% page-ref page="../untitled/62-63-64-path.md" %}

