# Tree 皇位/等差数列/Common Nodes

## **继承皇位 Monarchy**

Given an the following interface, implement its methods

```text
interface Monarchy {
  void birth(String child, String parent);
  void death(String name);
  List<String> getOrderOfSuccession();
}
```

```text
             king
          /         \
       a1            a2
      /  \          /  \
    b1   b2       c1    c2
   / \     \
d1    d2    d3
```

Order of Succession: king -&gt; a1 -&gt; b1 -&gt; d1 -&gt; d2 -&gt; b2 -&gt; d3 -&gt; a2 -&gt; c1 -&gt; c2

**Example:**

```text
Input: King(the first monarch) has 3 children Andy, Bob, Catherine. Andy has one child Matthew. Bob has two children Alex and Asha. Catherine has no children. 
Output: [King, Andy, Matthew, Bob, Alex, Asha, Catherine]
```

```python
class Monarch(object):
    def __init__(self):
        self.name = None
        self.childern = []
        self.isAlive = True
        
class Monarchy(object):
    def __init__(self):
        self.firstMonarch = None
        self.monarchs = {}
        
    def birth(self, child, parent):
        # create monarch
        monarch = Monarch()
        monarch.name = child
        # If it's the first monarch
        if(parent == None and self.firstMonarch == None):
            self.firstMonarch = monarch
        else:
            # find parent and add child
            if not (parent in self.monarchs):
                print "Parent not found"
                return
            self.monarchs[parent].childern.append(monarch)
        # Add the monarch to hash table
        self.monarchs[child] = monarch

    def death(self, name):
        self.monarchs[name].isAlive = False

    def preOrder(self, node):
        if node:
            if node.isAlive:
                # don't print dead people 
                print(node.name)
            for child in node.childern:
                self.preOrder(child)

    def getOrderOfSuccession(self):
        self.preOrder(self.firstMonarch)

# Main
m = Monarchy()
m.birth("king", None)
m.birth("Andy", "king")
m.birth("Bob", "king")
m.birth("Catherine", "king")
m.birth("Matthew" , "Andy")
m.birth("Alex " , "Bob")
m.birth("Asha " , "Bob")
m.getOrderOfSuccession()
```

## 二叉树最长等差数列

给一个二叉树，找出其所包含的最长的等差数列\(common diff可以&gt;0 / =0 / &lt;0\)的长度（注意只能从上往下找），我用了dfs。

```text
比如：
            2
        4       5
      6   3   2   1
  8  7 9 8 6 1 6  7
输出 4(2->4->6->8).
再比如：
          2
        4 5
      6 3 2 1
  7 7 9 8 6 1 6 7
输出 3(2->4->6).
```

Follow up: 如果也可以bottom up，怎么做。  
recursion：当前node看左右子树的diff 算length， 返回两个子树的长度，两个diff

```text
比如：
  4
2  6
输出3（2->4->6）
```

```python
def commonDifference(root):
    maxlen = 0
    stack = []
    if root.left:
        stack.append((root.left,root.left.val-root.val,2))
    if root.right:
        stack.append((root.right,root.right.val-root.val,2))

    while stack:
        node,comm,cur_len = stack.pop()
        if node.left:
            if node.left.val - node.val == comm:
                stack.append((node.left,comm,cur_len+1))
            else:
                maxlen = max(maxlen,cur_len)
                stack.append((node.left,node.left.val-node.val,2))
        if node.right:
            if node.right.val - node.val == comm:
                stack.append((node.right,comm,cur_len+1))
            else:
                maxlen = max(maxlen,cur_len)
                stack.append((node.right,node.right.val-node.val,2))

    return maxlen
```

## 

## Complete binary tree

题目是一个complete binary tree， 所有的node从上到下，从左到右都做了编号，求某个编号是否存在在这个树里

> > 因为左子树的index是parent index的两倍， 右子树的index是两倍加一， 用这个可以逐级推算出每个level的parent的index， 然后验证是否存在

```text

```

