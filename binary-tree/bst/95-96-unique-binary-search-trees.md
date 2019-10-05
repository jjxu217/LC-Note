# 95/96 Unique Binary Search Trees

## 96. Unique Binary Search Trees

Given _n_, how many structurally unique **BST's** \(binary search trees\) that store values 1 ... _n_?

**Example:**

```text
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

### Solution: DP

Define  
1. G\(n\): the number of unique BST for a sequence of length `n`.  
2. F\(i, n\): the number of unique BST, where the number `i` is served as the root of BST \(1≤i≤n\).

Base Case: G\(0\) = G\(1\) = 1  
Induction Rule: 

$$
G(n) = \sum_{i=1}^n F(i, n);  F(i, n) = G(i-1)G(n-i) \\
=> G(n) =  \sum_{i=1}^n  G(i-1)G(n-i)
$$

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [1] * (n + 1)
        for i in range(2, n + 1):
            dp[i] = sum(dp[i - j] * dp[j - 1] for j in range(1, i + 1))
        return dp[-1]
```

## 95. Unique Binary Search Trees II

Given an integer _n_, generate all structurally unique **BST's** \(binary search trees\) that store values 1 ... _n_.

**Example:**

```text
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
```

```python
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        def node(val, left, right):
            node = TreeNode(val)
            node.left = left
            node.right = right
            return node
        def trees(first, last):
            return [node(root, left, right)
                    for root in range(first, last+1)
                    for left in trees(first, root-1)
                    for right in trees(root+1, last)] or [None]
        return trees(1, n) if n >= 1 else []
        
        
    def generateTrees(self, n: int) -> List[TreeNode]:
        def generate(first, last):
            trees = []
            for root in range(first, last+1):
                for left in generate(first, root-1):
                    for right in generate(root+1, last):
                        node = TreeNode(root)
                        node.left, node.right = left, right
                        trees.append(node)
            return trees or [None]
        return generate(1, n) if n >= 1 else []
```

