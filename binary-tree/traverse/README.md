# Traverse

## 94. Inorder

### Recursion

```python
class Solution(object):
    def inorderTraversal(self, root):
        self.res = []
        self.inorder(root)
        return self.res
    def inorder(self, root):
        if not root:
            return
        self.inorder(root.left)
        self.res.append(root.val)
        self.inorder(root.right)
```

### Iteration

```python
    def inorderTraversal(self, root):
        res, stack = [], []
        while stack or root:
            if root:
                stack.append(root)
                root = root.left
            else:
                temp = stack.pop()
                res.append(temp.val)
                root = temp.right
        return res
```

## 144. Preorder

### Recursion

```python
class Solution(object):
    def preorderTraversal(self, root):
        self.res = []
        self.preorder(root)
        return self.res
    def preorder(self, root):
        if not root:
            return
        self.res.append(root.val)
        self.preorder(root.left)
        self.preorder(root.right)
```

### Iteration

```python
    def preorderTraversal(self, root):
        stack, res = [root], []
        while stack:
            node = stack.pop()
            if node:
                res.append(node.val)
                stack.append(node.right)
                stack.append(node.left)
        return res
```

## 144. Postorder

### Recursion

```python
class Solution(object):
    def postorderTraversal(self, root):
        self.res = []
        self.postorder(root)
        return self.res
    def postorder(self, root):
        if not root:
            return
        self.postorder(root.left)
        self.postorder(root.right)
        self.res.append(root.val)
```

### Iteration

```python
    def postorderTraversal(self, root):
        stack, res = [root], []
        while stack:
            node = stack.pop()
            if node:
                res.append(node.val)
                stack.append(node.left)
                stack.append(node.right)     
        return res[::-1]
```

## Iterative method conclude:

For in-order traversal:

```python
    def inorderTraversal(self, root):
        res, stack = [], [(0, root)]
        while stack:
            seen, node = stack.pop()
            if not node: continue
            if not seen:
                stack.extend([(0, node.right), (1, node), (0, node.left)]) 
            else: 
                res.append(node.val)
        return res
```

For pre-order traversal:

```python
    def preorderTraversal(self, root):
        res, stack = [], [(0, root)]
        while stack:
            seen, node = stack.pop()
            if not node: continue
            if not seen:
                stack.extend([(0, node.right), (0, node.left), (1, node)]) 
            else: 
                res.append(node.val)
        return res
```

For post-order traversal:

```python
    def postorderTraversal(self, root):
        res, stack = [], [(0, root)]
        while stack:
            seen, node = stack.pop()
            if not node: continue
            if not seen:
                stack.extend([(1, node), (0, node.right), (0, node.left)]) 
            else: 
                res.append(node.val)
        return res
```

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

