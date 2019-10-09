# 删边

## **二叉树删除边** LC684，685

**684. Redundant Connection: undirected graph**  
union-find  
iterate through the edges  
if not same root, connect  
if the same root already, redundant

**685. Redundant Connection: directed graph**  
1. 2 parents, no circle  
2. 2 parents, with a circle   
3. 1 parent, with a circle

```text
Check whether there is a node that has multiple parents.
  If yes, store 2 edges: edge 1, edge 2
    union-find on all edges except that 2 edges
          if edge 1 creates a circle, remove it
          else edge 2
If no, union-find and remove the edge that creating a circle
```

Given a binary tree, where an arbitary node has 2 parents i.e two nodes in the tree have the same child. Identify the defective node and remove an extra edge to fix the tree.

**Example:**

```text
Input:
	   1
	  / \
	 2   3
	/ \ /
   4   5

Output:

     1			       1
    / \			      / \
   2   3    or	     2   3
  / \ 			    /   /
 4   5		       4   5

Explanation: We can remove either 3-5 or 2-5.
```

```python
def removeEdgeBT(root):
	def dfs(root):
		if not root or root in seen: return None
		seen.add(root)
		root.left = dfs(root.left)
		root.right = dfs(root.right)
		return root
		
	seen = set()
	return dfs(root)
```

**Follow-up 1:**  
What if the tree is a BST?

```python
def removeEdgeBST(root):
	def dfs(root, lower, upper):
		if not root or root.val <= lower or root.val >= upper: return None
		root.left = dfs(root.left, lower, root.val)
		root.right = dfs(root.right, root.val, upper)
		return root
		
	return dfs(root, float('-inf'), float('inf'))
```

**Follow-up 2:**  
What if the tree is an N-ary tree?

```python
def removeEdgeNT(root):
	def dfs(root):
		if not root or root in seen: return None
		seen.add(root)
		idx = None
		for i, kid in enumerate(root.children):
			if kid in seen:
				idx = i
				break
			dfs(kid)
		if idx != None:
			root.children.pop(idx)
		return root
		
	seen = set()
	return dfs(root)
```

## See also Redundant connection

{% page-ref page="../../graph/dfs-and-union-find.md" %}

