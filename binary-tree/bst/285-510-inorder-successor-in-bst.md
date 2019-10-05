# 285/510 Inorder Successor in BST

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

