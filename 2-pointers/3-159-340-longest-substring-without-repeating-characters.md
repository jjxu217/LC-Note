# 窗口类 3/159/340/395 Longest Substring Without Repeating Characters

## 3. Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```text
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```text
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```text
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Sol: sliding window: 

Use a **dic** {used char: idx}. Iterate all char in s: if current char in dic, check the left bound `l = max(l, dic[ch] + 1)`,update res `res = max(res, i - l + 1)`

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l = res = 0
        dic = {}
        for i, ch in enumerate(s):
            if ch in dic:
                l = max(l, dic[ch] + 1)
            res = max(res, i - l + 1)
            dic[ch] = i
        return res
```

## 159. Longest Substring with At Most Two Distinct Characters

Given a string _**s**_ , find the length of the longest substring _**t**_  that contains **at most** 2 distinct characters.

**Example 1:**

```text
Input: "eceba"
Output: 3
Explanation: t is "ece" which its length is 3.
```

**Example 2:**

```text
Input: "ccaabbb"
Output: 5
Explanation: t is "aabbb" which its length is 5.
```

### Using a dict {char: idx}, record right most idx so far. 

If len\(dic\) == 3, update left side, and dict, delete the left most char. `del_idx = min(dic.values()); del dic[s[del_idx]]`

```python
class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: str) -> int:     
        l = res = 0
        dic = {}
        for i, ch in enumerate(s):
            dic[ch] = i
            if len(dic) == 3:
                del_idx = min(dic.values())
                del dic[s[del_idx]]
                l = del_idx + 1
            res = max(res, i - l + 1)
        return res
```

## 340. Longest Substring with At Most K Distinct Characters

Given a string, find the length of the longest substring T that contains at most k distinct characters.

**Use a dic to record {char: newest idx}, right pointer `i` iterate string s. When the length of dic is `k + 1`,  Find the smallest idx in dic, delete that idx in `dic` to make it contains less or equal to k distinct chars. Update the left pointer to the deleted idx + 1 `l = del_idx + 1`**

```python
#Time = O(k * n)
class Solution:
    def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        l = res = 0
        dic = {}
        for i, ch in enumerate(s):
            dic[ch] = i
            if len(dic) == k + 1:
                del_idx = min(dic.values())
                del dic[s[del_idx]]
                l = del_idx + 1
            res = max(res, i - l + 1)
        return res
```

### Or use OrderDict\(\), similar to LRU cache, time = O\(n\)

```python
def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        l = res = 0
        dic = collections.OrderedDict()
        for i, char in enumerate(s):
            if char in dic:
                del dic[char]
            dic[char] = i
            if len(dic) == k + 1:
                _, del_idx = dic.popitem(last=False)#把oldest(最左的idx)删掉
                l = del_idx + 1
            res = max(res, i - l + 1)
        return res
```

## 395. Longest Substring with At Least K Repeating Characters

Find the length of the longest substring **T** of a given string \(consists of lowercase letters only\) such that every character in **T** appears no less than k times.

**Example 1:**

```text
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```text
Input:
s = "ababbc", k = 2

Output:
5

The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

### count char in s, if the number of char is less than k, split s with that char, search recursively.

```python
class Solution:
    #for each char in s, if char repeat less than k time, 
    #split s by that char, and recursive find the result, choose the max
    def longestSubstring(self, s: str, k: int) -> int:
        for c in set(s):
            if s.count(c) < k:
                return max(self.longestSubstring(t, k) for t in s.split(c))
        return len(s)
```

## 

