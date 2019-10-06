# Palindrome

## 266. Palindrome Permutation

Given a string, determine if a permutation of the string could form a palindrome.

**Example 1:**

```text
Input: "code"
Output: false
```

**Example 3:**

```text
Input: "carerac"
Output: true
```

```python
class Solution:
    def canPermutePalindrome(self, s: str) -> bool:
        c = collections.Counter(s)
        return sum(v % 2 for v in c.values()) < 2
```

## 267. Palindrome Permutation II

Given a string `s`, return all the palindromic permutations \(without duplicates\) of it. Return an empty list if no palindromic permutation could be form.

**Example 1:**

```text
Input: "aabb"
Output: ["abba", "baab"]
```

**Example 2:**

```text
Input: "abc"
Output: []
```

```python
class Solution:
    def generatePalindromes(self, s: str) -> List[str]:
        def dfs(path):
            if len(path) == n:
                res.append(path)
                return
            for x in counter:
                if counter[x] > 0:
                    counter[x] -= 2
                    dfs(x + path + x)
                    counter[x] += 2
                    
        res = []    
        counter = collections.Counter(s)
        n = len(s)
        odd = [c for c, v in counter.items() if v % 2]
        if len(odd) > 1:
            return []
        elif len(odd) == 1:
            counter[odd[0]] -= 1
            dfs(odd[0])
        else:
            dfs('')
        return res
```

## 409. Longest Palindrome

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example `"Aa"` is not considered a palindrome here.

**Note:**  
Assume the length of given string will not exceed 1,010.

**Example:**

```text
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

### Note:

For each letter, say it occurs `v` times. We know we have **`v // 2 * 2`** letters that can be partnered for sure. For example, if we have `'aaaaa'`, then we could have `'aaaa'` partnered, which is `5 // 2 * 2 = 4` letters partnered.

At the end, if there was any `v % 2 == 1`, then that letter could have been a unique center. Otherwise, every letter was partnered. To perform this check, we will check for **`v % 2 == 1` and `ans % 2 == 0`**, the latter meaning we haven't yet added a unique center to the answer.

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        c = collections.Counter(s)
        ans = 0
        for v in c.values():
            ans += v // 2 * 2
            if ans % 2 == 0 and v % 2 == 1:
                ans += 1
        return ans
```

## 5. Longest Palindromic Substring

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```text
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```text
Input: "cbbd"
Output: "bb"
```

### Note: 中心开花

when the iteration for l/ r are over, the longest substring is s\[l+1, r\], note **the char at l and r are excluded.**

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        global_l = global_r =  l = 0
        for i in range(2 * len(s) - 1):
            l = i // 2
            r = l + i % 2 #string are even or odd, like "abba", "aba"
            while l >= 0 and r < len(s) and s[l] == s[r]
                l -= 1
                r += 1
            if r - l  - 1 > l:
                global_l, global_r = l, r
                l = r - l  - 1
        return s[global_l + 1: global_r]
```

## 674. Palindromic Substrings

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Example 1:**

```text
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

**Example 2:**

```text
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        res = 0
        for i in range(2 * len(s) - 1):
            l = i // 2
            r = l + i % 2
            while l >= 0 and r < len(s) and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
        return res
```

## 516. Longest Palindromic Subsequence

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

**Example 1:**  
Input:

```text
"bbbab"
```

Output:

```text
4
```

One possible longest palindromic subsequence is "bbbb".

### **Sol1: DP**

```python
dp[i][j] represent longest palindrome subsequence of s[i to j].
dp[i][j] = 2 + dp[i + 1][j - 1] if s[i] == s[j] else max(dp[i + 1][j], dp[i][j - 1])
```

```python
class Solution:   
    #Time = O(n ^ 2), Space = O(n ^ 2)
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[1] * n for _ in range(n)]
        for j in range(1, n):
            for i in reversed(range(0, j)):
                if s[i] == s[j]:
                    dp[i][j] = 2 + dp[i + 1][j - 1]  if i + 1 <= j - 1 else 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
        return dp[0][-1]
    #滚动数组： Time = O(n ^ 2), Space = O(n)
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [1] * n
        for j in range(1, n):
            pre = dp[j]
            for i in reversed(range(0, j)):
                temp = dp[i]
                if s[i] == s[j]:
                    dp[i] = 2 + pre  if i + 1 <= j - 1 else 2
                else:
                    dp[i] = max(dp[i + 1], dp[i])
                pre = temp
        return dp[0][-1]
```

### Sol2: DFS with memo

```python
def longestPalindromeSubseq(self, s: str) -> int:
        memo = {}
        
        def dfs(s):
            if s not in memo:
                L = 0
                for c in set(s):
                    l, r = s.find(c), s.rfind(c) #the left/right index of char c
                    L = max(L, 1 if l==r else 2 + dfs(s[l+1:r]))
                memo[s] = L
            return memo[s]
        
        return dfs(s)
```

## 214. Shortest Palindrome

Given a string _**s**_, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

**Example 1:**

```text
Input: "aacecaaa"
Output: "aaacecaaa"
```

**Example 2:**

```text
Input: "abcd"
Output: "dcbabcd"
```

### sol: Two pointers and recursion

use i to compare character from end of s and beginning of s. If it's equal, increment i by 1.   
The first part s\[i:\] might be Palindrome. The second part s\[i:\] is not Palindrome, we reverse s\[i:\] and insert to the beginning. do recursion.  
Time = O\(n^2\), worst case T\(n\)=T\(n−2\)+O\(n\)

```python
class Solution:
    def shortestPalindrome(self, s: str) -> str:
        if len(s) < 2: return s
        i = 0
        for j in reversed(range(len(s))):
            if s[j] == s[i]:
                i += 1
        if i == len(s):
            return s
        return s[i:][::-1] + self.shortestPalindrome(s[:i]) + s[i:]
```

### sol: find longest palindrome start from the beginning, use reversed string

Create the reverse of the original string s  
Iterate over the variable i from 0 to size\(s\)−1:   
If s\[0:n-i\] == rev\[i:\] return rev\[:i\] + s   
time = O\(n^2\)

```python
    def shortestPalindrome(self, s: str) -> str:
        if len(s) < 2: return s
        n = len(s)
        rev = s[::-1]
        for i in range(n):
            #find the longest Palindrome start from beginning
            if s[:n-i] == rev[i:]: 
                return rev[:i] + s
```

## 336. Palindrome Pairs

Given a list of **unique** words, find all pairs of **distinct** indices `(i, j)` in the given list, so that the concatenation of the two words, i.e. `words[i] + words[j]` is a palindrome.

**Example 1:**

```text
Input: ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]] 
Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]
```

**Example 2:**

```text
Input: ["bat","tab","cat"]
Output: [[0,1],[1,0]] 
Explanation: The palindromes are ["battab","tabbat"]
```

### Sol: time=O\(n \* w^2\), n being length of the list, w being the average word length\)

The basic idea is to check each word for prefixes \(and suffixes\) that are themselves palindromes. If you find a prefix that is a valid palindrome, then the suffix reversed can be paired with the word in order to make a palindrome. It's better explained with an example.

```text
words = ["bot", "t", "to"]
```

Starting with the string "bot". We start checking all prefixes. If `"", "b", "bo", "bot"` are themselves palindromes. The empty string and "b" are palindromes. We work with the corresponding suffixes \("bot", "ot"\) and check to see if their reverses \("tob", "to"\) are present in our initial word list. If so \(like the word to"to"\), we have found a valid pairing where the reversed suffix can be **prepended** to the current word in order to form "to" + "bot" = "tobot".

You can do the same thing by checking all suffixes to see if they are palindromes. If so, then finding all reversed prefixes will give you the words that can be **appended** to the current word to form a palindrome.

The process is then repeated for every word in the list. Note that when considering suffixes, we explicitly leave out the empty string to avoid counting duplicates. That is, if a palindrome can be created by appending an entire other word to the current word, then we will already consider such a palindrome when considering the empty string as prefix for the other word.

```python
class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        def is_palindrome(check):
            return check == check[::-1]
        dic = {word:i for i, word in enumerate(words)}
        res = []
        for i, word in enumerate(words):
            n = len(word)
            for j in range(n + 1):
                pref = word[:j]
                suf = word[j:]
                #case1: pref is pal, back + cur is pal
                if is_palindrome(pref):
                    back = suf[::-1]
                    if back != word and back in dic:
                        res.append([dic[back], i])
                #casae2: suf is pal, cur + back is pal
                #when j == n, pref==cur, suf=='',which will be checked by case1 of other word
                if j != n and is_palindrome(suf):
                    back = pref[::-1]
                    if back != word and back in dic:
                        res.append([i, dic[back]])
        return res
            
```

