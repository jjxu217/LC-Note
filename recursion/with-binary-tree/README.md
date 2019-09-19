# With Binary Tree

**Way of thinking:**

1. What do you expect from your lchild/rchild?
2. What do you want to do in the current layer
3. What do you want to report to your parents?

## 236. Lowest Common Ancestor of a Binary Tree

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        if root == None or root == p or root == q:
            return root
        l = self.lowestCommonAncestor(root.left, p, q)
        r = self.lowestCommonAncestor(root.right, p, q)
        if l and r:
            return root
        return l or r
```

Iteration:

[https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/discuss/65245/Iterative-Solutions-in-PythonC%2B%2B](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/discuss/65245/Iterative-Solutions-in-PythonC%2B%2B)

### 变种1：Assume we have parents pointers 

-&gt; **Use HashSet record the parents**

### 变种**2:** Lowest Common Ancestor of k nodes

```python
class Solution(object):
    def lowestCommonAncestorK(self, root, s: set():
        if root == None or root in s:
            return root
        l = self.lowestCommonAncestor(root.left, s)
        r = self.lowestCommonAncestor(root.right, s)
        if l and r:
            return root
        return l or r
```

### 变种3：Lowest Common Ancestor of k-nary Tree

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        if root == None or root == p or root == q:
            return root
        cnt = 0
        temp = None
        for c in root.children:                #step1
            node = self.lowestCommonAncestor(c, p, q)
            if node:                         #step2
                cnt += 1
                if cnt == 2:
                    return root
                temp = node
       return temp  #step3
```

## 235. Lowest Common Ancestor of a Binary Search Tree

```python
#iteration
def lowestCommonAncestor(self, root, p, q):
    while root:
        if max(p.val, q.val) < root.val:
            root = root.left
        elif min(p.val, q.val) > root.val:
            root = root.right
        else:
            return root
    return None
 
#recursion   
def lowestCommonAncestor(self, root, p, q):
    if max(p.val, q.val) < root.val :
        return self.lowestCommonAncestor(root.left, p, q)
    if min(p.val, q.val) > root.val:
        return self.lowestCommonAncestor(root.right, p, q)
    return root
```

## 104. Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Note:** A leaf is a node with no children.

**Example:**

Given binary tree `[3,9,20,null,null,15,7]`,

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

## 559. Maximum Depth of N-ary Tree

### Note:

**Base case: If node is None, return 0; if node.children is None, which is leaf node, return 1.**

```python
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        if not root.children:
            return 1
        maxD = 0
        for c in root.children:
            maxD = max(maxD, self.maxDepth(c))
        return maxD + 1
```

## 110. Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the depth of the two subtrees of _every_ node never differ by more than 1.

**Example 1:**

Given the following tree `[3,9,20,null,null,15,7]`:

```text
    3
   / \
  9  20
    /  \
   15   7
```

Return true.

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        
        def helper(root):
            if not root:
                return 0
            l, r = helper(root.left), helper(root.right)
            if l == -1 or r == -1 or abs(l - r) > 1:
                return -1
            return max(l, r) + 1 
        
        return helper(root) != -1
```

## 111. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example:**

Given binary tree `[3,9,20,null,null,15,7]`,

```text
    3
   / \
  9  20
    /  \
   15   7
```

### Note:

leaf node height is 1. If one node with one child, the height is the one child's height + 1.

```python
class Solution:
#DFS
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        l, r = self.minDepth(root.left), self.minDepth(root.right)
        if not l and not r:
            return 1
        elif not l or not r:
            return 1 + (l or r)
        return min(l, r) + 1
#BFS
    def minDepth(self, root: TreeNode) -> int:
        if not root: return 0
        queue = collections.deque([(root, 1)])
        while queue:
            node, d = queue.popleft()
            if not node.left and not node.right:
                return d
            if node.left: queue.append((node.left, d + 1))
            if node.right: queue.append((node.right, d + 1))
```

## 100. Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

**Example 1:**

```text
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```

**Example 2:**

```text
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```

```python
class Solution:
    #recursion
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True
        elif not p or not q:
            return False
        elif p.val != q.val:
            return False
        else:
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right) 
        
    def isSameTree(self, p, q):
        def check(p, q):
            # if both are None
            if not p and not q:
                return True
            # one of p and q is None
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True
        
        deq = deque([(p, q),])
        while deq:
            p, q = deq.popleft()
            if not check(p, q):
                return False
            
            if p:
                deq.append((p.left, q.left))
                deq.append((p.right, q.right))
                    
        return True
```

## 101. Symmetric Tree

Given a binary tree, check whether it is a mirror of itself \(ie, symmetric around its center\).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

```text
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following `[1,2,2,null,3,null,3]` is not:

```text
    1
   / \
  2   2
   \   \
   3    3
```

```python
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root: return True
        return self.helper(root.left, root.right)
    
    def helper(self, t1, t2):
        if not t1 and not t2:
            return True
        elif not t1 or not t2:
            return False
        elif t1.val != t2.val:
            return False
        return self.helper(t1.left, t2.right) and self.helper(t2.left, t1.right)
```

## 250. Count Univalue Subtrees

Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

**Example :**

```text
Input:  root = [5,1,5,5,5,null,5]

              5
             / \
            1   5
           / \   \
          5   5   5

Output: 4
```

```python
class Solution(object):
    def countUnivalSubtrees(self, root): 
        self.res = 0
        self.helper(root)
        return self.res
    
    def helper(self, root):
        if not root: 
            return True
#check if both children are univalue
        l, r = self.helper(root.left), self.helper(root.right)
#if children are univalue, and univalue for current node, then
        if l and r and (not root.left or root.left.val == root.val) and (not root.right or root.right.val == root.val):
            self.res += 1
            return True
        return False
```

