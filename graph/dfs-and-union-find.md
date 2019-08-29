# DFS & Union Find

```python
without rank
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

### Sol1: Union find, time=O\(\) with union-rank, N is the number of vertices \(and also the number of edges\) in the graph

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
        for edge in edges:
            dsu.union(*edge)
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
                dfs(i)
                res += 1
        return res
    
    #bfs
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        #build the graph
        graph = {x:[] for x in range(n)}
        for a, b in edges:
            graph[a].append(b)
            graph[b].append(a)
            
        res = 0
        for i in range(n):
            if i not in graph:
                continue
            res += 1
            queue = [i] #one group of component
            for j in queue:
                if j in graph:
                    queue += graph[j] #add neighbors
                    del graph[j]
        return res              
```

