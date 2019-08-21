# Construct

## 105. Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**  
You may assume that duplicates do not exist in the tree.

For example, given

```text
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```text
    3
   / \
  9  20
    /  \
   15   7
```

```python
class Solution:
    #in-place, HashMap to find val
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        def helper(in_left=0, in_right=len(inorder)):
            nonlocal pre_idx
            if in_left >= in_right: 
                return None
            val = preorder[pre_idx]
            root = TreeNode(val)
            idx = idx_map[val]
            
            pre_idx += 1
            root.left = helper(in_left, idx)
            root.right = helper(idx+1, in_right)
            return root
            
        # start from first preorder element
        pre_idx = 0
        # build a hashmap value -> its index
        idx_map = {val:idx for idx, val in enumerate(inorder)} 
        return helper()
        
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not inorder:
            return None
        val = preorder.pop(0)
        root = TreeNode(val)
        idx = inorder.index(val)
        root.left = self.buildTree(preorder, inorder[:idx])
        root.right = self.buildTree(preorder, inorder[idx+1:])
        return root 
    
    #iteration   
    def buildTree(self, preorder, inorder):
	# construct hashmap mapping a value to its inorder index
        idx_map = {val:idx for idx, val in enumerate(inorder)} 			
	# Iterate over preorder and construct the tree 
        stack = []
        head = None
        for val in preorder:
            if not head:
                head = TreeNode(val)
                stack.append(head)
            else:
                node = TreeNode(val)
                if idx_map[val] < idx_map[stack[-1].val]:
                    stack[-1].left = node
                else:
                    while stack and idx_map[stack[-1].val] < idx_map[val]:
                        u = stack.pop()
                    u.right = node
                stack.append(node)
        return head
```

## 106. Construct Binary Tree from Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.

For example, given

```text
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```text
    3
   / \
  9  20
    /  \
   15   7
```

### Note: in-order is better

```python
class Solution:
 #in-place
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        idx_map = {val: idx for idx, val in enumerate(inorder)}
        post_idx = len(postorder) - 1
        
        def helper(in_left, in_right):
            nonlocal post_idx
            if in_left >= in_right:
                return None
            val = postorder[post_idx]
            idx = idx_map[val]
            root = TreeNode(val)
           #post_index processed one element
            post_idx -= 1 
               
            root.right = helper(idx+1, in_right)
            root.left = helper(in_left, idx)
            return root
        
        return helper(0, len(inorder))
        
   def buildTree(self, inorder, postorder):
        if not inorder or not postorder:
            return None
        
        root = TreeNode(postorder.pop())
        inorderIndex = inorder.index(root.val)

        root.right = self.buildTree(inorder[inorderIndex+1:], postorder)
        root.left = self.buildTree(inorder[:inorderIndex], postorder)

        return root
```

## 1008. Construct Binary Search Tree from Preorder Traversal

Return the root node of a binary **search** tree that matches the given `preorder` traversal.

_\(Recall that a binary search tree is a binary tree where for every node, any descendant of `node.left` has a value `<` `node.val`, and any descendant of `node.right` has a value `>` `node.val`.  Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.\)_

**Example 1:**

```text
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

![](../../.gitbook/assets/image%20%2818%29.png)

```python
class Solution:
    def bstFromPreorder(self, preorder):
        pre_idx = 0
        n = len(preorder)
        
        def helper(lower=float('-inf'), upper=float('inf')):
            nonlocal pre_idx
            if pre_idx >= n or preorder[pre_idx] < lower or preorder[pre_idx] > upper:
                return None
            val = preorder[pre_idx]
            root = TreeNode(val)
            pre_idx += 1
            
            root.left = helper(lower, val)
            root.right = helper(val, upper)
            return root
        
        return helper()
```

