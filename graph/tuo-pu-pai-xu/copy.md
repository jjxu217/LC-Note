# Copy

## 138. Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list.

**Example 1:**

![](https://discuss.leetcode.com/uploads/files/1470150906153-2yxeznm.png)

```text
Input:
{"$id":"1","next":{"$id":"2","next":null,"random":{"$ref":"2"},"val":2},"random":{"$ref":"2"},"val":1}

Explanation:
Node 1's value is 1, both of its next and random pointer points to Node 2.
Node 2's value is 2, its next pointer points to null and its random pointer points to itself.
```

### Sol:

建立一个original node和copy node的一一对应关系

1. defaultdict: one pass: set  **copy\[None\] = None**  
2. two pass: first generate all node, then add the connection

```python
class Solution:
 #defaultdict,one pass 
    def copyRandomList(self, head: 'Node') -> 'Node':
        copy = collections.defaultdict(lambda: Node(0, None, None))
        copy[None] = None
        m = head
        while m:
            copy[m].val = m.val
            copy[m].next = copy[m.next]
            copy[m].random = copy[m.random]
            m = m.next
        return copy[head]
    
    #two pass: O(2n)
    def copyRandomList(self, head: 'Node') -> 'Node':
        copy = {None: None}
        m = n = head       
        while m:
            copy[m] = Node(m.val, None, None)
            m = m.next
        while n:
            copy[n].next = copy[n.next]
            copy[n].random = copy[n.random]
            n = n.next
        return copy[head]   
        
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head:
            return None
        m = {}
        newhead = Node(head.val, None, None)
        m[head] = newhead
        cur = newhead
        while head:
            if head.next:
                if head.next not in m:
                    m[head.next] = Node(head.next.val, None, None)                   
                cur.next = m[head.next]          
            if head.random:
                if head.random not in m:
                    m[head.random] = Node(head.random.val, None, None)
                cur.random = m[head.random]
            head = head.next
            cur = cur.next
        return newhead
```

## 133. Clone Graph

Given a reference of a node in a [**connected**](https://en.wikipedia.org/wiki/Connectivity_%28graph_theory%29#Connected_graph) undirected graph, return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) \(clone\) of the graph. Each node in the graph contains a val \(`int`\) and a list \(`List[Node]`\) of its neighbors.

**Example:**

![](https://assets.leetcode.com/uploads/2019/02/19/113_sample.png)

```text
Input:
{"$id":"1","neighbors":[{"$id":"2","neighbors":[{"$ref":"1"},{"$id":"3","neighbors":[{"$ref":"2"},{"$id":"4","neighbors":[{"$ref":"3"},{"$ref":"1"}],"val":4}],"val":3}],"val":2},{"$ref":"4"}],"val":1}

Explanation:
Node 1's value is 1, and it has two neighbors: Node 2 and 4.
Node 2's value is 2, and it has two neighbors: Node 1 and 3.
Node 3's value is 3, and it has two neighbors: Node 2 and 4.
Node 4's value is 4, and it has two neighbors: Node 1 and 3.
```

![](../../.gitbook/assets/image%20%2839%29.png)

```python
class Solution:
    #dfs
    def cloneGraph(self, node: 'Node') -> 'Node':
        mapping = collections.defaultdict(lambda : Node(0, []))
        stack = [node]
        
        while stack:
            cur = stack.pop() #expand node
            mapping[cur].val = cur.val #set value
            for nei in cur.neighbors:
                #generate nei, add to stack
                if nb not in mapping:
                    stack.append(nb)
                mapping[cur].neighbors.append(mapping[nb])  #set nei              
        return mapping[node]
        
    #bfs
    def cloneGraph(self, node: 'Node') -> 'Node':
        copy = collections.defaultdict(lambda: Node(0, []))
        queue = collections.deque([node])
        while queue:
            cur = queue.popleft() #expand node
            copy[cur].val = cur.val #set expanded node val        
            for nb in cur.neighbors: 
                #add the not-yet-generated node to queue
                if nb not in copy:
                    queue.append(nb) #generate node
                #set nei
                copy[cur].neighbors.append(copy[nb])
        return copy[node]
        
    #dfs recursion
    def cloneGraph(self, node: 'Node') -> 'Node':
        copy = collections.defaultdict(lambda: Node(0, []))
        
        def dfs(m):
            if m in copy:
                return copy[m]                
            copy[m].val = m.val
            for nb in m.neighbors:
                copy[m].neighbors.append(dfs(nb))
            return copy[m]
        
        dfs(node)
        return copy[node]
```

