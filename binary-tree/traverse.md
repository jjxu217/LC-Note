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
                stack.extend([(0, node.right), (1, node), (0, node.left), ]) 
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

