# Serialization

## 297. Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Example:** 

```text
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

**Clarification:** The above format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

### Note: Post-oder traverse, use '\#' representing None

if use recursion, post-order traverse it, and record the value. During deserialize,  post-order traverse can use pop\(\) to get the value, pre-order need pop\(0\).  
Or use queue for BFS traverse.

```python
class Codec:
    #post-order recursion
    def serialize(self, root):
        res = []
        
        def postorder(root):
            if not root:
                res.append('#')
                return
            postorder(root.left)
            postorder(root.right)
            res.append(str(root.val))
            
        postorder(root)
        return ' '.join(res)    

    def deserialize(self, data):   
        data = data.split()
       
        def helper():      
            val = data.pop()
            if val == '#':
                return None
            node = TreeNode(int(val))
            node.right = helper()
            node.left = helper()
            return node      
        
        return helper()
    
    #BFS
    def serialize(self, root):
        if not root: return ''
        queue = collections.deque([root])
        res = []
        while queue:
            node = queue.popleft()
            if not node:
                res.append('#')
            else:
                res.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
        return ' '.join(res)

    def deserialize(self, data):  
        if not data:
            return None
        data = data.split()
        root = TreeNode(int(data[0]))
        q = collections.deque([root])
        idx = 1
        while q:
            node = q.popleft()
            if data[idx] != '#':
                node.left = TreeNode(int(data[idx]))
                q.append(node.left)
            idx += 1
            
            if data[idx] != '#':
                node.right = TreeNode(int(data[idx]))
                q.append(node.right)
            idx += 1         
            
        return root
```

## 449. Serialize and Deserialize BST

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

```python
class Codec:
    def serialize(self, root):
        res = []
        def postorder(root):
            if not root:
                return
            postorder(root.left)
            postorder(root.right)
            res.append(str(root.val))
            
        postorder(root)
        return ' '.join(res)       

    def deserialize(self, data):      
        def helper(lower=float('-inf'), upper=float('inf')):
            if not data or data[-1] < lower or data[-1] > upper:
                return None
            val = data.pop()
            root = TreeNode(val)
            root.right = helper(val, upper)
            root.left = helper(lower, val)
            return root
        
        data = [int(x) for x in data.split()]
        return helper()
```

## 428. Serialize and Deserialize N-ary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following `3-ary` tree

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

as `[1 [3[5 6] 2 4]]`. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

### Sol: preorder traverse, append '\#' when no more children

```python
class Codec:
    def serialize(self, root):
        if not root: return ''
        
        def helper(root):
            if not root:
                return
            res.append(str(root.val))
            for c in root.children:
                helper(c)     
            res.append('#')  # indicates no more children, continue serialization from parent
        
        res = []
        helper(root)
        return ' '.join(res)

    def deserialize(self, data):
        if not data: 
            return None
        data = collections.deque(data.split())
        root = Node(int(data.popleft()), [])
        
        def helper(node):
            if not data: 
                return
            while data[0] != '#': # add child nodes with subtrees
                child = Node(int(data.popleft()), [])     
                node.children.append(child)
                helper(child)                       
            data.popleft()  # discard the "#"
        
        helper(root)
        return root
```

