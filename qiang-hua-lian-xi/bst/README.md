# BST

## 98. Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree \(BST\).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

**Example 1:**

```text
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```

```python
class Solution:
    def isValidBST(self, root: TreeNode, low=float('-inf'), high=float('inf')) -> bool:
        if not root:
            return True
        if high > root.val > low:
            return self.isValidBST(root.left, low, root.val) \
            and self.isValidBST(root.right, root.val, high)
```

## **270.** Closest Binary Search Tree Value

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

**Note:**

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

**Example:**

```text
Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4
```

```python
class Solution:
    def closestValue(self, root: TreeNode, target: float) -> int:
        self.res = root.val
        
        def helper(root):
            if not root:
                return
            if abs(root.val - target) < abs(self.res - target):
                self.res = root.val
            if root.val > target:
                helper(root.left)
            elif root.val < target:
                helper(root.right)
                
        helper(root)
        return self.res
```

## 700. Search in a Binary Search Tree

Given the root node of a binary search tree \(BST\) and a value. You need to find the node in the BST that the node's value equals the given value. Return the subtree rooted with that node. If such node doesn't exist, you should return NULL.

For example, 

```text
Given the tree:
        4
       / \
      2   7
     / \
    1   3

And the value to search: 2
```

You should return this subtree:

```text
      2     
     / \   
    1   3
```

In the example above, if we want to search the value `5`, since there is no node with value `5`, we should return `NULL`.

Note that an empty tree is represented by `NULL`, therefore you would see the expected output \(serialized tree format\) as `[]`, not `null`.

```python
class Solution:
    #recursion
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root or root.val == val:
            return root
        elif root.val > val:
            return self.searchBST(root.left, val)
        else:
            return self.searchBST(root.right, val)
            
        
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        while root:
            if root.val == val:
                return root
            elif root.val < val:
                root = root.right
            else:
                root = root.left
        return None
```

## 450. Delete Node in a BST

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference \(possibly updated\) of the BST.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Note:** Time complexity should be O\(height of tree\).

**Example:**

```text
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].
```

![](../../.gitbook/assets/image%20%2812%29.png)

```python
class Solution:
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        if not root:
            return None
        if root.val > key: # if key value is less than root value, find the node in the left subtree
            root.left = self.deleteNode(root.left, key)
        elif root.val < key: # if key value is greater than root value, find the node in right subtree
            root.right = self.deleteNode(root.right, key)
        else: #if we found the node (root.value == key), start to delete it
            if not root.left and not root.right: #if leaf Node, just delete it
                return None
            elif not root.left or not root.right: #if has one child, return child
                return root.left or root.right
            else: # if have 2 children, copy the minimum number in right sub-tree, delete the minimum number node
                m = root.right
                while m.left:
                    m = m.left
                root.val = m.val 
                root.right = self.deleteNode(root.right, m.val)  
        return root
```

## 701. Insert into a Binary Search Tree

Given the root node of a binary search tree \(BST\) and a value to be inserted into the tree, insert the value into the BST. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Note that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

For example, 

```text
Given the tree:
        4
       / \
      2   7
     / \
    1   3
And the value to insert: 5
```

You can return this binary search tree:

```text
         4
       /   \
      2     7
     / \   /
    1   3 5
```

This tree is also valid:

```text
         5
       /   \
      2     7
     / \   
    1   3
         \
          4
```

![](../../.gitbook/assets/image%20%289%29.png)

```python
class Solution:
     #recursion
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root: 
            return TreeNode(val)
        elif val > root.val:
            root.right = self.insertIntoBST(root.right, val)
        else:
            root.left = self.insertIntoBST(root.left, val)
        return root
        
    #iteration
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        parent, cur = None, root
        while cur:
            parent = cur
            if val > cur.val:      
                cur = cur.right
            else:
                cur = cur.left
        if not parent:
            return None
        elif val > parent.val:
            parent.right = TreeNode(val)
        else:
            parent.left = TreeNode(val)
        return root 
```

## 285. Inorder Successor in BST

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG)

```text
Input: root = [2,1,3], p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

```text
Input: root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation: There is no in-order successor of the current node, so the answer is null.
```

### **Idea:**

**Case1: root.val &gt; p.val**: update cur, go left child  
**Case2: root.val &lt; p.val:** don't update cur, go right child  
**Case3: root.val == p.val:** find the successor from sub-tree\(p.right\), else return cur

```python
class Solution:
    def inorderSuccessor(self, root: 'TreeNode', p: 'TreeNode') -> 'TreeNode':
        cur = None #previous large one
        while root:
            if root.val > p.val:
                cur = root
                root = root.left
            elif root.val < p.val:
                root = root.right
            else: #root == p
                if not root.right: #if p has not right child, return cur
                    return cur
                root = root.right #else, return the successor
                while root.left:
                    root = root.left
                return root
```

## 510. Inorder Successor in BST II

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

You will have direct access to the node but not to the root of the tree. Each node will have a reference to its parent node.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG)

```text
Input: 
root = {"$id":"1","left":{"$id":"2","left":null,"parent":{"$ref":"1"},"right":null,"val":1},"parent":null,"right":{"$id":"3","left":null,"parent":{"$ref":"1"},"right":null,"val":3},"val":2}
p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of Node type.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

```text
Input: 
root = {"$id":"1","left":{"$id":"2","left":{"$id":"3","left":{"$id":"4","left":null,"parent":{"$ref":"3"},"right":null,"val":1},"parent":{"$ref":"2"},"right":null,"val":2},"parent":{"$ref":"1"},"right":{"$id":"5","left":null,"parent":{"$ref":"2"},"right":null,"val":4},"val":3},"parent":null,"right":{"$id":"6","left":null,"parent":{"$ref":"1"},"right":null,"val":6},"val":5}
p = 6
Output: null
Explanation: There is no in-order successor of the current node, so the answer is null.
```

### Idea:

* if node.right, find the most left node in the right subtree of the node;
* otherwise, find a parent upward that contains the node in its left subtree \(and thus the node is the most right one in the subtree\).

```python
class Solution:
    def inorderSuccessor(self, node: 'Node') -> 'Node':
        if node.right:
            node = node.right
            while node.left:
                node = node.left
            return node
        
         while node.parent and node.parent.val < node.val:
            node = node.parent
        return node.parent
```

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

## 230. Kth Smallest Element in a BST

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

**Note:**  
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example 1:**

```text
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Follow up:**  
What if the BST is modified \(insert/delete operations\) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        def inorder(r):
            return inorder(r.left) + [r.val] + inorder(r.right) if r else []
        
        return inorder(root)[k - 1]
```

**Follow up**

> What if the BST is modified \(insert/delete operations\) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

[Insert](https://leetcode.com/articles/insert-into-a-bst/) and [delete](https://leetcode.com/articles/delete-node-in-a-bst/) in a BST were discussed last week, the time complexity of these operations is \mathcal{O}\(H\)O\(H\), where HH is a height of binary tree, and H = \log NH=logN for the balanced tree.

Hence without any optimisation insert/delete + search of kth element has \mathcal{O}\(2H + k\)O\(2H+k\) complexity. How to optimise that?

That's a design question, basically we're asked to implement a structure which contains a BST inside and optimises the following operations :

* Insert
* Delete
* Find kth smallest

Seems like a database description, isn't it? Let's use here the same logic as for [LRU cache](https://leetcode.com/articles/lru-cache/) design, and combine an indexing structure \(we could keep BST here\) with a double linked list.

Such a structure would provide:

* O\(H\) time for the insert and delete.
* O\(k\) for the search of kth smallest.

![bla](https://leetcode.com/problems/kth-smallest-element-in-a-bst/Figures/230/linked_list2.png)

The overall time complexity for insert/delete + search of kth smallest isO\(H+k\) instead of O\(2H+k\).

**Complexity Analysis**

* Time complexity for insert/delete + search of kth smallest:O\(H+k\), where HH is a tree height. O\(logN+k\) in the average case,O\(N+k\) in the worst case.
* Space complexity : O\(N\) to keep the linked list.

## 938. Range Sum of BST

Given a binary tree, find the largest subtree which is a Binary Search Tree \(BST\), where largest means subtree with largest number of nodes in it.

**Note:**  
A subtree must include all of its descendants.

**Example:**

```text
Input: [10,5,15,1,8,null,7]

   10 
   / \ 
  5  15 
 / \   \ 
1   8   7

Output: 3
Explanation: The Largest BST Subtree in this case is the highlighted one.
             The return value is the subtree's size, which is 3.
```

```python
class Solution:
    #recursion
    def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
        if not root:
            return 0
        s = 0
        if L <= root.val <= R:
            s += root.val 
        if root.val > L:
            s += self.rangeSumBST(root.left, L, R)
        if root.val < R:
            s += self.rangeSumBST(root.right, L, R)
        return s
        
    #iteration    
    def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
        res = 0
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                if L <= node.val <= R:
                    res += node.val
                if L < node.val:
                    stack.append(node.left)
                if node.val < R:
                    stack.append(node.right)
        return res
```

## 538. Convert BST to Greater Tree

Given a Binary Search Tree \(BST\), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

**Example:**

```text
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
```

### Idea:

traverse reversed in-order, visit the node in descending order   
self.prefix\_sum record the sum of all larger number

```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.prefix_sum = 0
        
        def traverse(root):
            if not root:
                return 
            traverse(root.right)
            self.prefix_sum += root.val
            root.val = self.prefix_sum
            traverse(root.left)            
         
        traverse(root)
        return root
```

## 333. Largest BST Subtree

Given a binary tree, find the largest subtree which is a Binary Search Tree \(BST\), where largest means subtree with largest number of nodes in it.

**Note:**  
A subtree must include all of its descendants.

**Example:**

```text
Input: [10,5,15,1,8,null,7]

   10 
   / \ 
  5  15 
 / \   \ 
1   8   7

Output: 3
Explanation: The Largest BST Subtree in this case is the highlighted one.
             The return value is the subtree's size, which is 3.
```

### Idea:

Need 3 parameters from the child: **child size, child min range, child max range**

```python
class Solution:
    def largestBSTSubtree(self, root: TreeNode) -> int:
        self.l = 0
        
        def dfs(root): #return 3 value: (cur_size, min_range, max_range)
            if not root:
                return (0, float('inf'), float('-inf'))
            l, r = dfs(root.left), dfs(root.right)        
            
            if l[0] >= 0 and r[0] >= 0 and l[2] < root.val < r[1]: 
                #if both sub-tree are BST and curent is BST
                cur_size = l[0] + r[0] + 1
                self.l = max(self.l, cur_size)
                return (cur_size, min(l[1], root.val), max(r[2], root.val))
            else:
                #if not BST, size as -1
                return (-1, 0, 0)
            
        dfs(root)
```

## 530. Minimum Absolute Difference in BST

Given a binary search tree with non-negative values, find the minimum [absolute difference](https://en.wikipedia.org/wiki/Absolute_difference) between values of any two nodes.

**Example:**

```text
Input:

   1
    \
     3
    /
   2

Output:
1

Explanation:
The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).
```

```python
class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        self.m = float('inf')
        
        def dfs(root):
            if not root:
                return (float('inf'), float('-inf'))
            l, r = dfs(root.left), dfs(root.right)
            self.m = min(self.m, root.val - l[1], r[0] - root.val)            
            return (min(root.val, l[0]), max(root.val, r[1]))
        
        dfs(root)
        return self.m
    
    #in-order traverse
    def getMinimumDifference(self, root: TreeNode) -> int:
        L = []
        def dfs(node):
            if node.left: dfs(node.left)
            L.append(node.val)
            if node.right: dfs(node.right)
        dfs(root)
        return min(abs(a - b) for a, b in zip(L, L[1:]))
```

## 501. Find Mode in Binary Search Tree

Given a binary search tree \(BST\) with duplicates, find all the [mode\(s\)](https://en.wikipedia.org/wiki/Mode_%28statistics%29) \(the most frequently occurred element\) in the given BST.

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
* The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
* Both the left and right subtrees must also be binary search trees.

For example:  
Given BST `[1,null,2,2]`,

```text
   1
    \
     2
    /
   2
```

return `[2]`.

**Note:** If a tree has more than one mode, you can return them in any order.

**Follow up:** Could you do that without using any extra space? \(Assume that the implicit stack space incurred due to recursion does not count\).

```python
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        c = collections.Counter()
        
        def helper(root):
            if not root:
                return
            helper(root.left)
            c[root.val] += 1    
            helper(root.right)
            
        helper(root)
        max_ct = max(c.values())
        return [k for k, v in c.items() if v == max_ct]
    
```

