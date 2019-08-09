# N-nary Traverse



## 429. N-ary Tree Level Order Traversalre

Given an n-ary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

For example, given a `3-ary` tree:

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

We should return its level order traversal:

```text
[
     [1],
     [3,2,4],
     [5,6]
]
```

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, children):
        self.val = val
        self.children = children
"""
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return[]
        res, level = [], [root]
        while level:
            res.append([node.val for node in level])
            level = [child for cur in level for child in cur.children]
        return res
```

