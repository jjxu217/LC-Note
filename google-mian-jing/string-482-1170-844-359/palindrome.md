# Palindrome

## 2个string palindrome

给两个string，相同长度，问能不能在同一个地方切一刀，然后两个string的各半边组成一个palindrome。 比如aabac和xyzaa，可以在aab\|ac和xyz\|aa这里切，然后aab和aa可以组成palindrome。要求是必须切在相同的位置，而且要第一个左边和第二个右边，或者第二个左边和第一个右边组成palindrome。  
followup是给出所有位置，小哥给了提示才用了two pointer做的

```python
def cut2string(s1, s2):
    n = len(s1)
    p1, p2 = 0, n - 1
    while p1 <= p2:
        if s1[p1] == s2[p2]:
            p1 += 1
            p2 -= 1
```

## 131. Palindrome Partitioning

Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.  
Return all possible palindrome partitioning of _s_.

**Example:**

```text
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

### DFS:

```python
    def partition(self, s):
        def dfs(s, path, res):
            if not s:
                res.append(path[:])
                return
            for i in range(1, len(s)+1):
                if s[:i] == s[i-1::-1]:
                    path.append(s[:i])
                    dfs(s[i:], path, res)
                    path.pop()        
        res = []
        dfs(s, [], res)
        return res
```



