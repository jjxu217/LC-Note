# 112/114/437/666/257/687/124/129 Path Sum

## 112. Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```text
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

```python
class Solution(object):
    def hasPathSum(self, root, sum):
        if not root: 
            return False
        elif not root.left and not root.right:
            return sum == root.val
        sum -= root.val
        return self.hasPathSum(root.left, sum) or self.hasPathSum(root.right, sum)
```

## 113. Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```text
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```

Return:

```text
[
   [5,4,11,2],
   [5,8,4,5]
]
```

```python
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        res = []
        
        def helper(root, path, sum):
            if not root:
                return
            elif not root.left and not root.right:
                if sum == root.val:
                    res.append(path + [root.val])
                return 
            sum -= root.val
            helper(root.left, path + [root.val], sum)
            helper(root.right, path + [root.val], sum)
        
        helper(root, [], sum)
        return res
```

## 437. Path Sum III

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards \(traveling only from parent nodes to child nodes\).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```text
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

### Idea: 

Construct a prefix sum, when traverse the tree, keep look back the previous prefix sum.

If use list to record the prefix sum, we can find the path, but the time to look up is O\(n\)

If we use a dict to record the prefix, the time to look up is O\(1\)

```python
class Solution:
    def pathSum(self, root: TreeNode, summ: int) -> int:
        
        def helper(root, prefix, summ):
            if not root:
                return 0
            # update prefix with cur value
            prefix.append(prefix[-1] + root.val)
            #count target sum path end at cur, including cur
            cnt = sum(prefix[-1] - prefix[i] == summ for i in range(len(prefix) - 1))
            l = helper(root.left, prefix, summ)
            r = helper(root.right, prefix, summ)
            prefix.pop()
            return cnt + l + r 
        
        return helper(root, [0], summ)
 
#use dict to record the prefix sum and frequency, so the    
    def pathSum(self, root: TreeNode,summ: int) -> int:
        self.res = 0
        
        def helper(root, summ, curPathSum, prefix):
            if not root:
                return
            # calculate currPathSum and required pastPathSum
            curPathSum += root.val
            pastPathSum = curPathSum - summ
            # update result and prefix
            self.res += prefix.get(pastPathSum, 0)
            prefix[curPathSum] = prefix.get(curPathSum, 0) + 1
            
            helper(root.left, summ, curPathSum, prefix)
            helper(root.right, summ, curPathSum, prefix)
            # when move to a different branch, the currPathSum is no longer available, hence remove one. 
            prefix[curPathSum] -= 1
            
        helper(root, summ, 0, {0: 1})
        return self.res
```

## 666. Path Sum IV

If the depth of a tree is smaller than `5`, then this tree can be represented by a list of three-digits integers.

For each integer in this list:

1. The hundreds digit represents the depth `D` of this node, `1 <= D <= 4.`
2. The tens digit represents the position `P` of this node in the level it belongs to, `1 <= P <= 8`. The position is the same as that in a full binary tree.
3. The units digit represents the value `V` of this node, `0 <= V <= 9.`

Given a list of `ascending` three-digits integers representing a binary tree with the depth smaller than 5, you need to return the sum of all paths from the root towards the leaves.

**Example 1:**

```text
Input: [113, 215, 221]
Output: 12
Explanation: 
The tree that the list represents is:
    3
   / \
  5   1

The path sum is (3 + 5) + (3 + 1) = 12.
```

**Example 2:**

```text
Input: [113, 221]
Output: 4
Explanation: 
The tree that the list represents is: 
    3
     \
      1

The path sum is (3 + 1) = 4.
```

```python
class Solution:
    #bottom up
    def pathSum(self, nums: List[int]) -> int:
        summ, leaf = {}, {}
        for i in nums[::-1]:
            d, p, v = i // 100, i // 10 % 10, i % 10
            #count leaf number of d,p node, and get the summ for sub-tree d, p
            leaf[d, p] = max(1, leaf.get((d + 1, 2 * p), 0) + leaf.get((d + 1, 2 * p - 1), 0))
            summ[d, p] = summ.get((d + 1, 2 * p), 0) + summ.get((d + 1, 2 * p - 1), 0) + v * leaf[d, p]
        return summ.get((1, 1), 0)
    
   #top down
    def pathSum(self, nums: List[int]) -> int:
        self.res = 0
        values = {(i // 100, i // 10 % 10): i % 10 for i in nums}
        
        def dfs(d, p, path_sum):
            if (d, p) not in values:
                return
            path_sum += values[(d, p)]
            #if leaf node, then res += path sum
            if (d + 1, 2 * p - 1) not in values and (d + 1, 2 * p) not in values:
                self.res += path_sum
            else:
                dfs(d + 1, 2 * p - 1, path_sum)
                dfs(d + 1, 2 * p, path_sum)
                
        dfs(1, 1, 0)
        return self.res
```

## Tree:

2 class:

1. 人字形path： 一般需要从下往上传integer value
2. from root path \(直上直下\)： **carry a 直上直下 path prefix while traversing the tree**, record the path from root to current node

## 257. Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

**Note:** A leaf is a node with no children.

**Example:**

```text
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```

```python
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        self.res = []
        
        def dfs(path, root):
            if not root:
                return 
            #path add cur node
            path.append(str(root.val))
            #if leaf node, add the path to res; else serach child node
            if not root.left and not root.right:
                self.res.append("->".join(path))
            else:
                dfs(path, root.left)
                dfs(path, root.right)
            #pop cur node
            path.pop()
            
        dfs([], root)
        return self.res
```

## 687. Longest Univalue Path

Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

The length of path between two nodes is represented by the number of edges between them.

**Example 1:**

**Input:**

```text
              5
             / \
            4   5
           / \   \
          1   1   5
```

**Output:** 2

**Example 2:**

**Input:**

```text
              1
             / \
            4   5
           / \   \
          4   4   5
```

**Output:** 2

```python
class Solution:
    def longestUnivaluePath(self, root: TreeNode) -> int:
        self.res = 0
        
        def helper(root):
            if not root:
                return 0
            #get length of the longest path from both children
            l, r = helper(root.left), helper(root.right)
            lp = rp = 0
            #if child exist and value equals cur, array length + 1
            if root.left and root.val == root.left.val:
                lp += l + 1
            if root.right and root.val == root.right.val:
                rp += r + 1
            self.res = max(self.res, lp + rp)
            #return the longest array length
            return max(lp, rp)
            
        
        helper(root)
        return self.res
```

## 124. Binary Tree Maximum Path Sum

Given a **non-empty** binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

**Example 1:**

```text
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```

**Example 2:**

```text
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

### Idea

1. What do we expect from lchild/rchild: Max single path in my left subtree\(end at the left child node\), if this value is negative, we discard it Max single path in my right subtree\(end at the right child node\), if this value is negative, we discard it
2. What do you want to do in the current layer? update global\_max = left + right + root.value if feasible
3. What do you want to report to your parent?  it is usually the return type of the recursion function

```python
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        self.res = float('-inf')
        
        def dfs(root):
            if not root:
                return 0
            l, r = dfs(root.left), dfs(root.right)
            self.res = max(self.res, l + r + root.val)
            return max(root.val + max(l, r), 0)
        
        dfs(root)
        return self.res
```

## 129. Sum Root to Leaf Numbers

Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path `1->2->3` which represents the number `123`.

Find the total sum of all root-to-leaf numbers.

**Note:** A leaf is a node with no children.

**Example 1:**

```text
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

### Idea:

\(叶节点到叶节点\): 最长的Path, current node 算path length， 向parent传一条边的length

```python
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        self.res = 0
        
        def dfs(root, summ):
            if not root:
                return 0
            elif not root.left and not root.right:
                self.res += 10 * summ + root.val
            dfs(root.left, 10 * summ + root.val)
            dfs(root.right, 10 * summ + root.val)
            
        dfs(root, 0)
        return self.res
```

## 988. Smallest String Starting From Leaf

Given the `root` of a binary tree, each node has a value from `0` to `25` representing the letters `'a'` to `'z'`: a value of `0` represents `'a'`, a value of `1` represents `'b'`, and so on.

Find the lexicographically smallest string that starts at a leaf of this tree and ends at the root.

_\(As a reminder, any shorter prefix of a string is lexicographically smaller: for example, `"ab"` is lexicographically smaller than `"aba"`.  A leaf of a node is a node that has no children.\)_

1. 
**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/30/tree1.png)

```text
Input: [0,1,2,3,4,3,4]
Output: "dba"
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/30/tree2.png)

```text
Input: [25,1,3,1,3,0,2]
Output: "adz"
```

```python
class Solution:
    def smallestFromLeaf(self, root: TreeNode) -> str:
        self.res = []
        
        def dfs(root, path):
            if not root:
                return          
            path.append(root.val)
            #if leaf node, compare 
            if not root.left and not root.right:
                if not self.res or self.res[::-1] > path[::-1]:
                    self.res = path[:]
            else: 
                dfs(root.left, path)
                dfs(root.right, path)
            path.pop()
         
            
        dfs(root, [])        
        # convert the num path from root to leaf to string
        return ''.join([chr(ord('a') + i) for i in self.res[::-1]] ) 
```

## 

