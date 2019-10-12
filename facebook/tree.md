# Tree

{% page-ref page="../recursion/with-binary-tree/222.-count-complete-tree-nodes.md" %}

**Follow-up:**  
Given a complete **ternary tree**, count the number of nodes.

```text
class TreeNode {
	TreeNode left;
	TreeNode mid;
	TreeNode right;
}
```

**Example:**  
![](https://i.imgur.com/Ao4N9cb.png)





## Common Nodes in Two Binary Search Trees

Given two Binary Search Trees, find common nodes in them. In other words, find intersection of two BSTs.

**Example:**  
[![tree](https://media.geeksforgeeks.org/wp-content/cdn-uploads/tree5.png)](https://media.geeksforgeeks.org/wp-content/cdn-uploads/tree5.png)

 **\(Linear Time and O\(h\) Space\)** We can find common elements in O\(n\) time and O\(h1 + h2\) extra space where h1 and h2 are heights of first and second BSTs respectively.  
The idea is to use [iterative inorder traversal](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/). We use two auxiliary stacks for two BSTs. Since we need to find common elements, whenever we get same element, we print it.

```python
def Common(root1, root2):      
    # Create two stacks for two inorder traversals  
    s1, s2 = [], []
    res = []
  
    while True:         
        # append the Nodes of first tree in stack s1  
        while root1: 
            s1.append(root1) 
            root1 = root1.left  
        # append the Nodes of second tree in stack s2  
        while root2: 
            s2.append(root2) 
            root2 = root2.left 
  
        # Both root1 and root2 are NULL here  
        if s1 and s2: 
            root1, root2 = s1[-1], s2[-1]  
  
            # If current keys in two trees are same  
            if root1.key == root2.key: 
                res.appemd(root1.key)
                s1.pop(); s2.pop()           
                # move to the inorder successor  
                root1 = root1.right  
                root2 = root2.right 
  
            elif root1.key < root2.key:                
                # if root1.key < root2.key  
                # then consider s1 inorder successors 
                # root2 set to None, avoid append node to s2 twice
                s1.pop() 
                root1 = root1.right 
                root2 = None
                
            elif root1.key > root2.key: 
                s2.pop() 
                root2 = root2.right  
                root1 = None
        # Both roots and both stacks are empty  
        else: 
            return res 
```

##  **Find first pair of mismatching nodes**

Return first pair of mismatching nodes \(first pair as in **in-order**\) given two pre-order traversal arrays of BSTs.

**Example 1:**

```text
Input: pre1 = [5, 4, 2, 4, 8, 6, 9], pre2 = [5, 3, 2, 4, 8, 7, 9]
Output: [4, 3]
Explanation:
Tree 1:
	 5
  4     8
2  4   6  9

Tree 2:
	 5
  3     8
2  4   7  9

inorder1 = [2, 4, 4, 5, 6, 8, 9]
inorder2 = [2, 3, 4, 5, 7, 8, 9] 
```

**Example 2:**

```text
Input: pre1 = [2, 1, 3], pre2 = [1, 2]
Output: [3, null]
Explanation:
Tree 1:
  2
1   3

Tree 2:
	1
	   2

inorder1 = [1, 2, 3]
inorder2 = [1, 2]
```

**Example 3:**

```text
Input: pre1 = [2, 1, 3], pre2 = [1, 2, 3]
Output: []
Explanation:
Tree 1:
	2
  1   3

Tree 2:
	1
	   2
		  3

inorder1 = [1, 2, 3]
inorder2 = [1, 2, 3]
There is no mismatch because the in-order sequence for both is exactly the SAME, despite the trees are structurally different.
```

```python
def find_mismatch(root1, root2):      
    # Create two stacks for two inorder traversals  
    s1, s2 = [], []
  
    while True:         
        # append the Nodes of first tree in stack s1  
        while root1: 
            s1.append(root1) 
            root1 = root1.left  
        # append the Nodes of second tree in stack s2  
        while root2: 
            s2.append(root2) 
            root2 = root2.left 
  
        # get teh in-order element        
        root1 = s1.pop() if s1 else None
        root2 = s2.pop() if s2 else None
  
      #如果两个栈都是空，说明一样，返回[]
        if not root1 and not root2:
            return []
      #如果有一个栈为空，或者两个in-order 元素不一样，返回
        elif not root1 or not root2 or root1.val != root2.val:
            return [root1.val if root1 else None, root2.val if root2 else None]
      #如果root都不为空，而且值一样，看下一个
        else:
            root1 = root1.right
            root2 = root2.right
```

{% page-ref page="../stack/173.-binary-search-tree-iterator.md" %}

\*\*\*\*[**https://leetcode.com/discuss/interview-question/371618/Facebook-or-Onsite-or-Tree-Iterator**](https://leetcode.com/discuss/interview-question/371618/Facebook-or-Onsite-or-Tree-Iterator)\*\*\*\*

**Follow-up 1:**  
Assume `BSTIterator` is a **library** class. You need to extend the functionality of this class by implementing 2 methods:

```text
public boolean hasPrev() {
}

/** Can only take 1 step back */
public Integer prev() {
}
```

**Example:**

```text
       7
     /   \
    3    15
        /  \
       9    20

ExtendedBSTIterator it = new ExtendedBSTIterator(root);
it.hasNext(); // true
it.next(); // 3
it.next(); // 7
it.next(); // 9
it.next(); // 15
it.hasPrev(); // true
it.prev(); // 9
it.hasPrev(); // false bacause we can only move 1 step back
it.next(); // 15
it.next(); // 20
it.hasNext(); // false
it.hasPrev(); // true
it.prev(); // 15
it.hasNext(); // true
it.next(); // 20
```

Hint

**Follow-up 2:**  
What if you can move `k` steps back?

