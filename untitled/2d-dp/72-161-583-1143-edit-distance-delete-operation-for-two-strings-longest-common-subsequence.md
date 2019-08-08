# 72/161/583/1143 Edit Distance/Delete Operation for Two Strings/Longest Common Subsequence

Given two words _word1_ and _word2_, find the minimum number of operations required to convert _word1_ to _word2_.

You have the following 3 operations permitted on a word:

1. Insert a character
2. Delete a character
3. Replace a character

**Example 1:**

```text
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

**Example 2:**

```text
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

### Solution: DP, time=O\(m\*n\)

2维 DP \(m+1\) \* \(n+1\) matrix   
M\[i\]\[j\] = M\[i-1\]\[j-1\] if s1\[i-1\] == s2\[j-1\]   
              min\(M\[i-1\]\[j-1\], M\[i\]\[j-1\],M\[i-1\]\[j\]\) + 1

```python
class Solution:
    def minDistance(self, s: str, t: str) -> int:
        m, n = len(s), len(t)
            
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range(n + 1):
            for j in range(m + 1):
                if i == 0 or j == 0:
                    dp[i][j] = i + j
                elif s[j - 1] == t[i - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(dp[i][j], dp[i][j - 1], dp[i - 1][j]) + 1
        return dp[-1][-1]
    
    #DFS, TLE
    def minDistance(self, s: str, t: str) -> int:
        if not s: return len(t) 
        if not t: return len(s) 
        nothing = float('inf')
        if s[0] == t[0]:
            nothing = self.minDistance(s[1:], t[1:])
        insert = 1 + self.minDistance(s, t[1:])
        delete = 1 + self.minDistance(s[1:], t)
        replace = 1 + self.minDistance(s[1:], t[1:])
        return min(nothing, insert, delete, replace)
```

## 583. Delete Operation for Two Strings

Given two words word1 and word2, find the minimum number of steps required to make word1 and word2 the same, where in each step you can delete one character in either string.

**Example 1:**  


```text
Input: "sea", "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
```

### Solution: DP, time=O\(m\*n\)

2维 DP \(m+1\) \* \(n+1\) matrix   
M\[i\]\[j\] = M\[i-1\]\[j-1\] if s1\[i-1\] == s2\[j-1\]   
              min\( M\[i\]\[j-1\],M\[i-1\]\[j\]\) + 1

```python
class Solution:
    def minDistance(self, s: str, t: str) -> int:
        m, n = len(s), len(t)   
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        
        for i in range(n + 1):
            for j in range(m + 1):
                if i == 0 or j == 0:
                    dp[i][j] = i + j
                elif s[j - 1] == t[i - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(dp[i][j - 1], dp[i - 1][j]) + 1
        return dp[-1][-1]
    
#O(n) space, 滚动数组
    def minDistance(self, s: str, t: str) -> int:
        m, n = len(s), len(t)      
        dp = [0] * (m + 1)
        for i in range(n + 1):
            temp = [0] * (m + 1)
            for j in range(m + 1):       
                if i == 0 or j == 0:
                    temp[j] = i + j
                elif s[j - 1] == t[i - 1]:
                    temp[j] = dp[j - 1]
                else:
                    temp[j] = min(temp[j - 1], dp[j]) + 1
            dp = temp
        return dp[-1]
```

## 1143. Longest Common Subsequence

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A _subsequence_ of a string is a new string generated from the original string with some characters\(can be none\) deleted without changing the relative order of the remaining characters. \(eg, "ace" is a subsequence of "abcde" while "aec" is not\). A _common subsequence_ of two strings is a subsequence that is common to both strings.

If there is no common subsequence, return 0.

**Example 1:**

```text
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```text
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```text
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

### Solution: DP, time=O\(m\*n\)

2维 DP \(m+1\) \* \(n+1\) matrix   
M\[i\]\[j\] = M\[i-1\]\[j-1\] + 1 if s1\[i-1\] == s2\[j-1\]   
              **max**\( M\[i\]\[j-1\],M\[i-1\]\[j\]\) 

```python
class Solution:
    def longestCommonSubsequence(self, s: str, t: str) -> int:
        m, n = len(s), len(t)   
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range(1, n + 1):
            for j in range(1, m + 1):
                if s[j - 1] == t[i - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]) 
        return dp[-1][-1]
```

## 161. One Edit Distance

Given two strings **s** and **t**, determine if they are both one edit distance apart.

**Note:** 

There are 3 possiblities to satisify one edit distance apart:

1. Insert a character into _**s**_ to get _**t**_
2. Delete a character from _**s**_ to get _**t**_
3. Replace a character of _**s**_ to get _**t**_

**Example 1:**

```text
Input: s = "ab", t = "acb"
Output: true
Explanation: We can insert 'c' into s to get t.
```

**Example 2:**

```text
Input: s = "cab", t = "ad"
Output: false
Explanation: We cannot get t from s by only one step.
```

**Example 3:**

```text
Input: s = "1203", t = "1213"
Output: true
Explanation: We can replace '0' with '1' to get t.
```

```python
class Solution:
    def isOneEditDistance(self, s, t):
        m, n = len(s), len(t)
        if m > n: # force s no longer than t
            return self.isOneEditDistance(t, s)
        if n - m > 1 or s == t:
            return False
        for i in range(m):
            if s[i] != t[i]:
                return s[i+1:] == t[i+1:] or s[i:] == t[i+1:] #replace/insert
        return True
```

