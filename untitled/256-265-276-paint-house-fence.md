# 256/265/276 Paint House/Fence

## 256. Paint House

There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

**Note:**  
All costs are positive integers.

**Example:**

```text
Input: [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue. 
             Minimum cost: 2 + 5 + 3 = 10.
```

### Idea:

Base case: dp\[0\] = costs\[0\]  
Induction Rule: dp\[j\] = \[costs\[j\]\[i\] + min\(dp\[j - 1\]\[:i\] + dp\[j - 1\]\[i + 1:\]\) for i in range\(3\)\]

```python
class Solution:
    def minCost(self, costs: List[List[int]]) -> int:
       if not costs: return 0
        n, k = len(costs), 3
        for i in range(1, n):
            min1 = min(costs[i - 1])
            idx = costs[i - 1].index(min1)
            min2 = min(costs[i - 1][:idx] + costs[i - 1][idx + 1:])
            for j in range(k):
                if j == idx:
                    costs[i][j] += min2
                else:
                    costs[i][j] += min1
        return min(costs[-1])
    
    #O(1) space
    def minCost(self, costs):
        prev = [0] * 3
        for now in costs:
            prev = [now[i] + min(prev[:i] + prev[i+1:]) for i in range(3)]
        return min(prev)
```

## 265. Paint House II

The cost of painting each house with a certain color is represented by a `n x k` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color 0; `costs[1][2]` is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

**Note:**  
All costs are positive integers.

**Example:**

```text
Input: [[1,5,3],[2,9,4]]
Output: 5
Explanation: Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
             Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5. 
```

**Follow up:**  
Could you solve it in O\(nk\) runtime?

```python
class Solution:
    #Time = O(n *k)
    def minCostII(self, costs: List[List[int]]) -> int:
        if not costs: return 0
        n, k = len(costs), len(costs[0])
        for i in range(1, n):
            min1 = min(costs[i - 1])
            idx = costs[i - 1].index(min1)
            min2 = min(costs[i - 1][:idx] + costs[i - 1][idx + 1:])
            for j in range(k):
                if j == idx:
                    costs[i][j] += min2
                else:
                    costs[i][j] += min1
        return min(costs[-1])
    
#Time = O(n * k * k)
#corner case k == 1, in which, min(prev[:i] + prev[i+1:]) for i in range(k) take no variable
    def minCostII(self, costs: List[List[int]]) -> int:
        if not costs or not costs[0]:
            return 0
        n, k = len(costs), len(costs[0])
        if k == 1:
            return costs[0][0]
        prev = [0] * k
        for now in costs:
            prev = [now[i] + min(prev[:i] + prev[i+1:]) for i in range(k)]
        return min(prev)
```

## 276. Paint Fence

There is a fence with n posts, each post can be painted with one of the k colors.

You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.

**Note:**  
n and k are non-negative integers.

**Example:**

```text
Input: n = 3, k = 2
Output: 6
Explanation: Take c1 as color 1, c2 as color 2. All possible ways are:

            post1  post2  post3      
 -----      -----  -----  -----       
   1         c1     c1     c2 
   2         c1     c2     c1 
   3         c1     c2     c2 
   4         c2     c1     c1  
   5         c2     c1     c2
   6         c2     c2     c1
```

### Idea:

If n == 1, there would be k-ways to paint.

If n == 2, there would be two situations:

* 2.1 You paint same color with the previous post: k \* 1 ways to paint, named it as `same`
* 2.2 You paint differently with the previous post: k \* \(k-1\) ways to paint this way, named it as `dif`

So, you can think, if n &gt;= 2, you can always maintain these two situations,  
`You either paint the same color with the previous one, or differently`.

```python
class Solution:
    def numWays(self, n: int, k: int) -> int:
        if not n: return 0
        if n == 1: return k
        same, diff = k, k * (k - 1)
        for i in range(2, n):
            same, diff = diff, (same + diff) * (k - 1)
        return same + diff
```

