# 无向图

## 261. Graph Valid Tree

Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges \(each edge is a pair of nodes\), write a function to check whether these edges make up a valid tree.

**Example 1:**

```text
Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true
```

**Example 2:**

```text
Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
```

### Sol1: Union-Find

```python
class DSU:
    def __init__(self, n):
        self.p = list(range(n))
    
    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]
        
    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        self.p[xr] = yr
        return True
    
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        dsu = DSU(n)
        for a, b in edges:
            #如果a,b已经union了，说明有环，return false
            if dsu.find(a) == dsu.find(b):
                return False
            dsu.union(a, b)
        return all(dsu.find(0) == dsu.find(i) for i in range(n)) #all elements are connected
```

### Sol2: BFS/DFS: 设状态indicator\(是否expand过\)，先构造图；expand node\(如果expand过了，返回false\), generate neighbor node

```python
#bfs
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        state = [0] * n
        queue = collections.deque([0])
        e = collections.defaultdict(set)
        for a, b in edges:
            e[a].add(b)
            e[b].add(a)
        while queue:
            #expand cur node
            cur = queue.popleft()
            #如果expand过了，说明有环
            if state[cur] == 1:
                return False
            state[cur] = 1
            #生成相邻node
            for nei in e[cur]:
                e[nei].remove(cur)
                queue.append(nei)
            e.pop(cur)
        return all(state)
        
    #dfs
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        visited = [0] * n
        stack = [0]
        e = collections.defaultdict(set)
        for a, b in edges:
            e[a].add(b)
            e[b].add(a)
        while stack:
            cur = stack.pop()
            if visited[cur] == 1:
                return False
            visited[cur] = 1
            for nei in e[cur]:
                e[nei].remove(cur)
                stack.append(nei)
            e.pop(cur)
        return all(visited)
```

## 310. Minimum Height Trees

### Idea:

**对于一个形状和链表一样的图，我们知道最优解一定是中点；**

* **在这个 graph 里，我们要找的，其实是一层一层剥开最外围 nodes 之后，最里面的点。**

**对于 undirected graph： degree = "in"-degree + "out"-degree， degree == 1表示leaves  
There're at most two nodes can be Minimum Height Trees**

#### So we can iterative remove leaves and related edges till we reach 1 or 2.

```python
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
        #build the graph
        graph = collections.defaultdict(set)
        for a, b in edges:
            graph[a].add(b)
            graph[b].add(a)
      
        todo = set(range(n))
        while len(todo) > 2:
            leafs = set(i for i in todo if len(graph[i]) == 1)
            todo -= leafs
            for cur in leafs:
                for nei in graph[cur]:
                    graph[nei].remove(cur)
        return list(todo)
```

