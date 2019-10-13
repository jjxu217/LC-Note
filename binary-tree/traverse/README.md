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
        while True:
            if root:
                stack.append(root)
                root = root.left
            elif stack:
                temp = stack.pop()
                res.append(temp.val)
                root = temp.right
            else:
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

## Morris traversal for inorder 

```text
1. Initialize current as root 
2. While current is not NULL
   If the current does not have left child
      a) Print current’s data
      b) Go to the right, i.e., current = current->right
   Else
      a) Make current as the right child of the rightmost 
         node in current's left subtree
      b) Go to this left child, i.e., current = current->left
```

![](../../.gitbook/assets/image%20%2836%29.png)

```python
# Iterative function for inorder tree traversal 
def MorrisTraversal(root): 
      
    # Set current to root of binary tree 
    current = root  
      
    while current : 
          
        if not current.left: 
            print current.data
            current = current.right 
        else: 
            # Find the inorder predecessor of current 
            pre = current.left 
            while pre.right and pre.right != current: 
                pre = pre.right 
   
            # Make current as right child of its inorder predecessor 
            if not pre.right: 
                pre.right = current 
                current = current.left 
                  
            # Revert the changes made in if part to restore the  
            # original tree i.e., fix the right child of predecessor 
            else: 
                pre.right = None
                print current.data 
                current = current.right 
```

## 

