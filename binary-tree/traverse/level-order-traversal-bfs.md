# Level Order Traversal\(BFS\)

## 102. Level Order Traversal

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

For example:  
Given binary tree `[3,9,20,null,null,15,7]`,  


```text
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

```text
[
  [3],
  [9,20],
  [15,7]
]
```

```python
class Solution(object):
#list comprehension
    def levelOrder(self, root):
        if not root: return []
        res, level = [], [root]
        while level:
            res.append([node.val for node in level])            
            level = [kid for n in level for kid in (n.left, n.right) if kid]
        return res
     
#deque
    def levelOrder(self, root):
        if not root: return []
        q = collections.deque([root])   
        res = []
        while q:
            level = []
            width = len(q)
            for i in range(width):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            res.append(level)
        return res   
```

## 103. Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. \(ie, from left to right, then right to left for the next level and alternate between\).

For example:  
Given binary tree `[3,9,20,null,null,15,7]`,  


```text
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:  


```text
[
  [3],
  [20,9],
  [15,7]
]
```

```python
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root: return []
        res, level = [], [root]
        #left to right indicator
        l2r = 1
        while level:
            if l2r:
                res.append([node.val for node in level]) 
            else:
                res.append([node.val for node in level[::-1]])             
            level = [kid for n in level for kid in (n.left, n.right) if kid]                
            l2r ^= 1
        return res
        
#deque
    def zigzagLevelOrder(self, root):
        if not root: return []
        q = collections.deque([root])   
        res = []
        l2r = 1
        while q:
            level = []
            width = len(q)
            for i in range(width):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            if l2r:
                res.append(level)
            else:
                res.append(level[::-1])
            l2r ^= 1
        return res 
```

## 107.  Level Order Traversal II

Given a binary tree, return the bottom-up level order traversal of its nodes' values. \(ie, from left to right, level by level from leaf to root\).

return its bottom-up level order traversal as:

```text
[
  [15,7],
  [9,20],
  [3]
]
```

#### Reverse the solution above

```python
class Solution(object):
    def levelOrderBottom(self, root):
        if not root: return []
        res, level = [], [root]
        while level:
            res.append([node.val for node in level])            
            level = [kid for n in level for kid in (n.left, n.right) if kid]
        return res[::-1]
```

## 199. Binary Tree Right Side View

Given a binary tree, imagine yourself standing on the _right_ side of it, return the values of the nodes you can see ordered from top to bottom.

**Example:**

```text
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

### Sol: BFS level traverse

```python
#BFS: List conprehension
def rightSideView(self, root: TreeNode) -> List[int]:
    if not root: return []
    res, level = [], [root]
    while level:
        res.append(level[-1].val)
        level = [kid for n in level for kid in (n.left, n.right) if kid]
    return res
#queue
def rightSideView(self, root: TreeNode) -> List[int]:
    if not root: return []
    q = collections.deque([root])   
    res = []
    while q:
        res.append(q[-1].val)
        width = len(q)
        for i in range(width):
            node = q.popleft()
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)         
    return res  
```

### Sol: DFS, build a dict with \(depth: val\)

```python
def rightSideView(self, root):
    rightmost_value_at_depth = dict() # depth -> node.val
    max_depth = -1

    stack = [(root, 0)] #node and depth
    while stack:
        node, depth = stack.pop()
        if node:
            # maintain knowledge of the number of levels in the tree.
            max_depth = max(max_depth, depth)

            # only insert into dict if depth is not already present.                
            if depth not in rightmost_value_at_depth:
                rightmost_value_at_depth[depth] = node.val

            stack.append((node.left, depth+1))
            stack.append((node.right, depth+1))

    return [rightmost_value_at_depth[depth] for depth in range(max_depth+1)]
```

## 958. Check Completeness of a Binary Tree

Given a binary tree, determine if it is a _complete binary tree_.

**Definition of a complete binary tree from** [**Wikipedia**](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**:**  
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-1.png)

```text
Input: [1,2,3,4,5,6]
Output: true
Explanation: Every level before the last is full (ie. levels with node-values {1} and {2, 3}), and all nodes in the last level ({4, 5, 6}) are as far left as possible.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/15/complete-binary-tree-2.png)

```text
Input: [1,2,3,4,5,null,7]
Output: false
Explanation: The node with value 7 isn't as far left as possible.
```

```python
class Solution:
    def isCompleteTree(self, root: TreeNode) -> bool:
        if not root:
            return False
        q = collections.deque([root])
        while q:
            width = len(q)
            for i in range(width):
                node = q.popleft()
                if not node:
                    return not any(i for i in q)
                q.append(node.left)
                q.append(node.right)
```

