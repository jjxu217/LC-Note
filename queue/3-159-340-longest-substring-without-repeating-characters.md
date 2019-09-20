# 3/159/340 Longest Substring Without Repeating Characters 438. Find All Anagrams in a String

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

### Sol: sliding window

Use a **dic** to record the used char and the position, use **l** to record the left side of non-repeat char.

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

```python
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

## 438. Find All Anagrams in a String

Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```text
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```text
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

### Sliding windows

```python
class Solution:
    def findAnagrams(self, s2: str, s1: str) -> List[int]:
        res = []
        c1, c2 = collections.Counter(s1), collections.Counter(s2[:len(s1) - 1])
        for i in range(len(s1) - 1, len(s2)):
            c2[s2[i]] += 1
            if c2 == c1:
                res.append(i - len(s1) + 1) 
            c2[s2[i - len(s1) + 1]] -= 1
            if c2[s2[i - len(s1) + 1]] == 0:
                del c2[s2[i - len(s1) + 1]]
        return res
```

## 30. Substring with Concatenation of All Words

You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring\(s\) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

**Example 1:**

```text
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**

```text
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```

```python
class Solution(object):
    def findSubstring(self, s, words):
        if not words or not words[0]: return[]
        c = collections.Counter(words)
        m, n = len(words), len(words[0])
        res = []
        
        #Loop over word length
        for k in range(n):
            left = k
            subd = collections.Counter()
            cnt = 0
            #Loop over the string
            for j in range(k, len(s) - n + 1, n):
                word = s[j:j+n]
                #check if it is a valid word
                if word in c:
                    subd[word] += 1
                    cnt += 1
                    #if we got more cur word than needed
                    #move left pointer to the right of the last cur word, remove extra word for dict                 
                    while subd[word] > c[word]:
                        subd[s[left:left+n]] -= 1
                        left += n
                        cnt -= 1
                    #Count will be equal to m only when we all the words are read the exact number of times needed
                    if cnt == m:
                        res.append(left)
                #If is not a valid word then just skip over cur word
                else:
                    left = j + n
                    subd = collections.Counter()
                    cnt = 0
        return res
```

