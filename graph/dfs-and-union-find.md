# Union Find

```python
#without rank: Nlog(N)
class DSU:
    def __init__(self):
        self.p = list(range(1001))
    
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
#dict 
class DSU():
    def __init__(self):
        self.p = {}
        
    def find(self,x):
        self.p.setdefault(x, x)
        if x != self.p[x]:
            self.p[x] = self.find(self.p[x])
        return self.p[x]
    
    def union(self, x, y):
        self.p.setdefault(x, x)
        self.p.setdefault(y, y)
        xr, yr = self.find(x), self.find(y)
        self.p[xr] = yr

#with rank
class DSU(object):
    def __init__(self):
        self.p = list(range(1001))
        self.rnk = [0] * 1001

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        elif self.rnk[xr] < self.rnk[yr]:
            self.p[xr] = yr
        elif self.rnk[xr] > self.rnk[yr]:
            self.p[yr] = xr
        else:
            self.p[yr] = xr
            self.rnk[xr] += 1
        return True
```

## 721. Accounts Merge

Given a list `accounts`, each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in any order.

**Example 1:**  


```text
Input: 
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
Output: [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
Explanation: 
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```

### SOL1： DFS

* Time Complexity: `O(∑a_i ​loga_i​)`, where ai​ is the length of `accounts[i]`. Without the log factor, this is the complexity to build the graph and search for each component. The log factor is for sorting each component at the end.
* Space Complexity: `O(∑a_i​)`, the space used by our graph and our search.

```python
def accountsMerge(self, accounts):       
        em_to_name = {}
        graph = collections.defaultdict(set)
        # Build up the graph.
        for acc in accounts:
            name = acc[0]
            for email in acc[1:]:
                graph[acc[1]].add(email)
                graph[email].add(acc[1])
                em_to_name[email] = name

        # Perform DFS on graph and add to results.
        seen = set()
        ans = []
        for email in graph:
            if email not in seen:
                seen.add(email)
                stack = [email]
                component = []
                while stack:
                    cur = stack.pop()
                    component.append(cur)
                    for nei in graph[cur]:
                        if nei not in seen:
                            seen.add(nei)
                            stack.append(nei)
                ans.append([em_to_name[email]] + sorted(component))
        return ans
```

### SOL2: Union Find

* Time Complexity: O\(AlogA\), where A = \sum a\_i, and a\_i is the length of `accounts[i]`. If we used union-by-rank, this complexity improves to O\(Aα\(A\)\)≈O\(A\), where α is the _Inverse-Ackermann_ function.
* Space Complexity: O\(A\), the space used by our DSU structure.

```python

class Solution():
    def accountsMerge(self, accounts):
        dsu = DSU()
        em_to_name = {}
        em_to_id = {}
        i = 0
        for acc in accounts:
            name = acc[0]
            for email in acc[1:]:
                em_to_name[email] = name
                if email not in em_to_id:
                    em_to_id[email] = i
                    i += 1
                dsu.union(em_to_id[acc[1]], em_to_id[email])

        ans = collections.defaultdict(list)
        for email in em_to_name:
            ans[dsu.find(em_to_id[email])].append(email)

        return [[em_to_name[v[0]]] + sorted(v) for v in ans.values()]
```

## 684. Redundant Connection

In this problem, a tree is an **undirected** graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes \(with distinct values 1, 2, ..., N\), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[u, v]` with `u < v`, that represents an **undirected** edge connecting nodes `u` and `v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge `[u, v]` should be in the same format, with `u < v`.

**Example 1:**  


```text
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3
```

**Example 2:**  


```text
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]
Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

### Sol1: Union find, time=O\(N\) with union-rank, N is the number of vertices \(and also the number of edges\) in the graph

```python
class Solution(object):
    def findRedundantConnection(self, edges):
        dsu = DSU()
        for edge in edges:
            if not dsu.union(*edge):
                return edge
```

### Sol2: DFS, time=O\(N^2\). In the worst case, for every edge we include, we have to search every previously-occurring edge of the graph.

```python
    def findRedundantConnection(self, edges):
        #find if s/t are connected already
        def dfs(s, t):
            if s not in seen:
                seen.add(s)
                if s == t:
                    return True
                return any(dfs(nei, t) for nei in graph[s])
                 
        graph = collections.defaultdict(set)
        for s, t in edges:
            seen = set()
            if s in graph and t in graph and dfs(s,t):
                return [s, t]
            #add edge s-t
            graph[s].add(t)
            graph[t].add(s)
```

## 547. Friend Circles

There are **N** students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a **direct** friend of B, and B is a **direct** friend of C, then A is an **indirect** friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a **N\*N** matrix **M** representing the friend relationship between students in the class. If M\[i\]\[j\] = 1, then the ith and jth students are **direct** friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

**Example 1:**

```text
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
```

```python
class Solution:
    def findCircleNum(self, M: List[List[int]]) -> int:
        dsu = DSU()
        N = len(M)
        for i in range(1, N):
            for j in range(0, i):
                if M[i][j] == 1:
                    dsu.union(i, j)
        circles = set()
        for i in range(N):
            idx = dsu.find(i)
            circles.add(idx)
        return len(circles)
```

## 947. Most Stones Removed with Same Row or Column

On a 2D plane, we place stones at some integer coordinate points.  Each coordinate point may have at most one stone.

Now, a _move_ consists of removing a stone that shares a column or row with another stone on the grid.

What is the largest possible number of moves we can make?

**Example 1:**

```text
Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
Output: 5
```

**Example 2:**

```text
Input: stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
Output: 3
```

**Example 3:**

```text
Input: stones = [[0,0]]
Output: 0
```

### **数岛屿问题**

如果两个石头在同行或者同列，两个石头就是连接的。连在一起的石头，可以组成一个连通图。每一个连通图至少会剩下1个石头。  
所以我们希望存在一种思路，每个连通图都只剩下1个石头。  
这样这题就转化成了数岛屿的问题。

### **优化行列处理**

实际上我们对行列的搜索，没有任何本质区别。只不过是因为同一个index，可能是行也可能是列，所以我们做了区分。实际上，只要我们能区分行列的index，代码就可以缩减一半了。行的index我们还可以用0～N - 1，列的index我们使用N～2N-1就可以了

```python
class Solution:
    def removeStones(self, stones: List[List[int]]) -> int:
        dsu = DSU()
        for i, j in stones:
            dsu.union(i, j + 10000)
        return len(stones) - len({dsu.find(x) for x,y in stones})
```

## 323. Number of Connected Components in an Undirected Graph

Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges \(each edge is a pair of nodes\), write a function to find the number of connected components in an undirected graph.

**Example 1:**

```text
Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4 

Output: 2
```

**Example 2:**

```text
Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
```

### Sol: Union/DFS/BFS

```python
class Solution:
    #union find
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        dsu = DSU()
        for a, b in edges:
            dsu.union(a, b)
        res = set()
        for i in range(n):
            res.add(dsu.find(i))
        return len(res)
    
    #dfs
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        def dfs(i):
            if visited[i]:
                return 
            visited[i] = 1
            for nei in graph[i]:
                dfs(nei)
            
        visited = [0] * n
        #build the graph
        graph = collections.defaultdict(list)
        for a, b in edges:
            graph[a].append(b)
            graph[b].append(a)
            
        res = 0
        for i in range(n):
            if visited[i] == 0:
                res += 1
                dfs(i)           
        return res
    
    #bfs, 把queue 换成stack就是dfs
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        visited = [0] * n
        #build the graph
        graph = collections.defaultdict(list)
        for a, b in edges:
            graph[a].append(b)
            graph[b].append(a)
            
        res = 0
        for i in range(n):
            if not visited[i]:
                res += 1
                queue = collections.deque([i]) #one group of component
                while queue:
                    cur = queue.popleft()
                    if not visited[cur]:
                        visited[cur] = 1
                        queue += graph[cur] #add neighbors
                        del graph[cur]
        return res                
```

## 305. Number of Islands II

A 2d grid map of `m` rows and `n` columns is initially filled with water. We may perform an addLand operation which turns the water at position \(row, col\) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**

```text
Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]
```

**Explanation:**

Initially, the 2d grid `grid` is filled with water. \(Assume 0 represents water and 1 represents land\).

```text
0 0 0
0 0 0
0 0 0
```

Operation \#1: addLand\(0, 0\) turns the water at grid\[0\]\[0\] into a land.

```text
1 0 0
0 0 0   Number of islands = 1
0 0 0
```

Operation \#2: addLand\(0, 1\) turns the water at grid\[0\]\[1\] into a land.

```text
1 1 0
0 0 0   Number of islands = 1
0 0 0
```

Operation \#3: addLand\(1, 2\) turns the water at grid\[1\]\[2\] into a land.

```text
1 1 0
0 0 1   Number of islands = 2
0 0 0
```

Operation \#4: addLand\(2, 1\) turns the water at grid\[2\]\[1\] into a land.

```text
1 1 0
0 0 1   Number of islands = 3
0 1 0
```

### Idea: use a dict for parent

```python
class DSU:
    def __init__(self):
        self.p = {}
        self.count = 0
        
    def find(self, x):
        if x != self.p[x]:
            self.p[x] = self.find(self.p[x])
        return self.p[x]   
    
    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr != yr:
            self.count -= 1
            self.p[yr] = xr
            
    def setParent(self, x):
        if x not in self.p: 
            self.p[x] = x
            self.count += 1
        
class Solution:
    def numIslands2(self, m: int, n: int, positions: List[List[int]]) -> List[int]:
        dsu = DSU()
        ans = []
        for po in positions:
            idx = po[0]*n+po[1]
            dsu.setParent(idx)
            for di in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                x, y = po[0]+di[0], po[1]+di[1]
                if 0 <= x < m and 0 <= y < n and x*n+y in dsu.p:
                    dsu.union(idx, x*n+y)
            ans.append(dsu.count)
        return ans
```

