# 207/210 Course Schedule

## 207. Course Schedule

There are a total of n courses you have to take, labeled from `0` to `n-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?

**Example 1:**

```text
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```text
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        pre, sub = collections.defaultdict(set), collections.defaultdict(set)
        for a, b in prerequisites:
            pre[a].add(b)
            sub[b].add(a)
        free = set(range(numCourses)) - set(pre.keys())
        cnt = 0
        while free:
            cur = free.pop()
            cnt += 1
            for nxt in sub[cur]:
                pre[nxt].remove(cur)
                if not pre[nxt]:
                    free.add(nxt)
            sub.pop(cur)
        return cnt == numCourses
```

## 210. Course Schedule II

Return the topological order

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        res = []
        pre, sub = collections.defaultdict(set), collections.defaultdict(set)
        for a, b in prerequisites:
            pre[a].add(b)
            sub[b].add(a)
        free = set(range(numCourses)) - set(pre.keys())
        while free:
            cur = free.pop()
            res.append(cur)
            for nxt in sub[cur]:
                pre[nxt].remove(cur)
                if not pre[nxt]:
                    free.add(nxt)
            sub.pop(cur)
        return res if len(res) == numCourses else []
```

