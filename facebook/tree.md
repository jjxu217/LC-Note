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
    s1 = [] 
    s2 = [] 
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

