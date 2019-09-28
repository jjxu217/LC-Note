# Convert BST

## 108. Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of _every_ node never differ by more than 1.

**Example:**

```text
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if len(nums) == 0:
            return None
        mid = len(nums) // 2
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])
        return root
    
    #in-place
    def sortedArrayToBST(self, nums):
        def convert(left, right):
            if left > right:
                return None
            mid = (left + right) // 2
            node = TreeNode(nums[mid])
            node.left = convert(left, mid - 1)
            node.right = convert(mid + 1, right)
            return node
        return convert(0, len(nums) - 1)
```

## 109. Convert Sorted List to Binary Search Tree

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of _every_ node never differ by more than 1.

**Example:**

```text
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

### Solution: recursively find the mid ListNode, Time O\(nlogn\), Space\(Ologn\)

Base case: head is None or head is one Node  
Recursive rule: Find the mid Node, connect left/right

here we get the middle point,   
even case, like '1234', slow points to '3', '12' belongs to left, '4' is right   
odd case, like '12345', slow points to '3', '12' belongs to left, '3' is root, '45' belongs to right

```python
class Solution:
    def sortedListToBST(self, head):
        if not head:
            return 
        if not head.next:
            return TreeNode(head.val)
         
        slow, fast, pre = head, head, None
        while fast and fast.next:
            pre, slow, fast = slow, slow.next, fast.next.next
        pre.next = None
        root = TreeNode(slow.val)
        root.left = self.sortedListToBST(head)
        root.right = self.sortedListToBST(slow.next)
        return root
    
#or we can convert linkedlist to array, then convert to bst, space=time=O(n)
```

## 426. Convert Binary Search Tree to Sorted Doubly Linked List

Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

Let's take the following BST as an example, it may help you understand the problem better: 

![](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

We want to transform this BST into a circular doubly linked list. Each node in a doubly linked list has a predecessor and successor. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

The figure below shows the circular doubly linked list for the BST above. The "head" symbol means the node it points to is the smallest element of the linked list. 

![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

Specifically, we want to do the transformation in place. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. We should return the pointer to the first element of the linked list.

The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship. 

![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturnbst.png)

### Idea:

**Key points**: When traverse to current node, we need to know which node was the previous node; Never ever lost the control of the head node

```python
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        pre = None
        head = None
        
        def inorder(root):
            nonlocal head, pre 
            if not root: return
            inorder(root.left)
            if not head: #if first node, set as head
                head = root
            else: #connect cur node with pre node
                root.left = pre
                pre.right = root
            pre = root
            inorder(root.right)
            
        if not root:
            return None
        inorder(root)
        head.left = pre  #connect head and tail
        pre.right = head
        return head
```

## 114. Flatten Binary Tree to Linked List

Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

```text
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:

```text
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

### Solution:

1. Iteration: use stack
2. recursion: reversed in-order traverse

```python
class Solution:
    def flatten(self, root: TreeNode) -> None:
        if not root:
            return root
        pre = root
        stack = [root.right, root.left]
        while stack:
            node = stack.pop()
            if not node:
                continue
            pre.right = node
            pre.left = None
            stack.append(node.right)
            stack.append(node.left)
            pre = node
    
    #reversed in-order                
    def __init__(self):
        self.nxt = None
    
    def flatten(self, root):
        if not root:
            return None
        self.flatten(root.right)
        self.flatten(root.left)
    
        root.right, root.left = self.nxt, None
        self.nxt = root     
```

