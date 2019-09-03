# directed graph

## 997. Find the Town Judge

In a town, there are `N` people labelled from `1` to `N`.  There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

1. The town judge trusts nobody.
2. Everybody \(except for the town judge\) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given `trust`, an array of pairs `trust[i] = [a, b]` representing that the person labelled `a` trusts the person labelled `b`.

If the town judge exists and can be identified, return the label of the town judge.  Otherwise, return `-1`.

**Example 2:**

```text
Input: N = 3, trust = [[1,3],[2,3]]
Output: 3
```

**Example 3:**

```text
Input: N = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1
```

**Example 4:**

```text
Input: N = 3, trust = [[1,2],[2,3]]
Output: -1
```

### Sol: for the directed graph, The point with `in-degree - out-degree = N - 1` become the judge

```python
class Solution:
    #Consider trust as a Directed graph
    #The point with in-degree - out-degree = N - 1 become the judge.
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        cnt = [0] * (N + 1)
        for a, b in trust:
            cnt[a] -= 1
            cnt[b] += 1
        for i in range(1, N + 1):
            if cnt[i] == N - 1:
                return i
        return -1
```

## 277. Find the Celebrity

Suppose you are at a party with `n` people \(labeled from `0` to `n - 1`\) and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity \(or verify there is not one\) by asking as few questions as possible \(in the asymptotic sense\).

You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`. There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/02/02/277_example_1_bold.PNG)

```text
Input: graph = [
  [1,1,0],
  [0,1,0],
  [1,1,1]
]
Output: 1
Explanation: There are three persons labeled with 0, 1 and 2. graph[i][j] = 1 means person i knows person j, otherwise graph[i][j] = 0 means person i does not know person j. The celebrity is the person labeled as 1 because both 0 and 2 know him but 1 does not know anybody.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2019/02/02/277_example_2.PNG)

```text
Input: graph = [
  [1,0,1],
  [1,1,0],
  [0,1,1]
]
Output: -1
Explanation: There is no celebrity.
```

```python
class Solution(object):
    def findCelebrity(self, n):
        # The resulting candidate must be a celebrity by the following logic
        # If there is a celebrity, they must
        # a) be known by every person before them (thus, end up as "cand" at some point)
        # b) not know anyone after them (thus, never relinquish "cand")
        cand = 0
        for i in range(n):
            if knows(cand, i):
                cand = i
                
        # We've established that cand does not know anyone after it
        # Let's establish that cand is known by, but does not know everyone before it
        for i in range (cand):
            if knows(cand, i) or not knows(i, cand):   
                return -1

        # Make sure everyone after cand know cand
        for i in range(cand, n):
            if not knows(i, cand):
                return -1
        return cand
```

