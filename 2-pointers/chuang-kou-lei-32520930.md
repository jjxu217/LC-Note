# 窗口类 325/209/30

## 209. Minimum Size Subarray Sum

Given an array of **n** positive integers and a positive integer **s**, find the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.

**Example:** 

```text
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

**Follow up:**If you have figured out the O\(n\) solution, try coding another solution of which the time complexity is O\(n log n\). 

```python
class Solution:
    #sliding window
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        total = left = 0
        result = len(nums) + 1
        for right, n in enumerate(nums):
            total += n
            while total >= s:
                result = min(result, right - left + 1)
                total -= nums[left]
                left += 1
        return result if result <= len(nums) else 0
    
    #Time = O(n log n): prefix sum + bisect
    def minSubArrayLen(self, target, nums):
        result = len(nums) + 1
        for i in range(1, len(nums)):
            nums[i] += nums[i - 1]
        left = 0
        for right, n in enumerate(nums):
            if n >= target:
                left = self.find_left(left, right, nums, target, n)
                result = min(result, right - left + 1)
        return result if result <= len(nums) else 0

    def find_left(self, left, right, nums, target, n):
        while left < right:
            mid = (left + right) // 2
            if n - nums[mid] >= target:
                left = mid + 1
            else:
                right = mid
        return left
```

## 325. Maximum Size Subarray Sum Equals k

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

**Note:**  
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

**Example 1:**

```text
Input: nums = [1, -1, 5, -2, 3], k = 3
Output: 4 
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
```

**Example 2:**

```text
Input: nums = [-2, -1, 2, 1], k = 1
Output: 2 
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
```

**Follow Up:**  
Can you do it in O\(n\) time?

### Sol： prefix sum, look up with HashTable

```python
class Solution:      
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        res = 0
        dic = {0: -1}
        prefix = 0
        for i, n in enumerate(nums):
            prefix += n
            if prefix not in dic:#if current acc not in dic, add it;旧的不用更新
                dic[prefix] = i
            if prefix - k in dic: #if find the subarray, find the length
                res = max(res, i - dic[prefix - k])
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
        if not words or not words[0]: return []
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

