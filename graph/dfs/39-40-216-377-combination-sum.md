# 39/40/216/377 Combination Sum

## 39. Combination Sum

Given a **set** of candidate numbers \(`candidates`\) **\(without duplicates\)** and a target number \(`target`\), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

* All numbers \(including `target`\) will be positive integers.
* The solution set must not contain duplicate combinations.

**Example 1:**

```text
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```text
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

### Sol1:DFS, each level represents amount of a candidate number to select, level number == len\(candidate\)

```python
class Solution:
    #each level represents amount of a candidate number to select, level number == len(candidate)
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        self.res = []
        candidates.sort(reverse=True)
        self.dfs(0, [], target, candidates)
        return self.res
    
    def dfs(self, idx, path, remain, candidates):
        if idx == len(candidates):
            if remain == 0:
                self.res.append(path)
            return
        for i in range(remain // candidates[idx] + 1):
            self.dfs(idx + 1, path + [candidates[idx]] * i, remain - candidates[idx] * i, candidates)           
```

### Sol2: DFS, each level represents which candidate number to select. Use index to record current number position, only use the number after current, each level has len\(candidate\) child 

```python
 #each level represents which candidate number to select, use idx to de-duplicate
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        self.res = []
        candidates.sort(reverse=True)
        self.dfs(0, [], target, candidates)
        return self.res
    
    def dfs(self, idx, path, remain, candidates):
        if remain < 0:
            return
        if remain == 0:
            self.res.append(path)
            return      
        for i in range(idx, len(candidates)):
            self.dfs(i, path + [candidates[i]], remain - candidates[i], candidates)

```

## 40. Combination Sum II

Each number in `candidates` may only be used **once** in the combination.

**Example 1:**

```text
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

### Solution: DFS, binary tree: each level represents a candidate number is selected or not, level number == len\(candidate\)

same method to de-duplication, see **Subset II**

```python
while idx + 1 < len(candidates) and candidates[idx + 1] == candidates[idx]:
            idx += 1
```

```python
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        self.res = []    
        candidates.sort(reverse=True)
        self.dfs(0, [], target, candidates)
        return self.res
    
    def dfs(self, idx, path, remain, candidates):
        if idx == len(candidates):
            if remain == 0:
                self.res.append(path)
            return           
        
        #if select, prune if remain - candidates[idx] < 0
        if remain - candidates[idx] >= 0:
            self.dfs(idx + 1, path + [candidates[idx]], remain - candidates[idx], candidates)
        
        #if not selected, all same number will not be selected (handle duplication)
        while idx + 1 < len(candidates) and candidates[idx + 1] == candidates[idx]:
            idx += 1
        self.dfs(idx + 1, path, remain, candidates)
```

### Solution2: ****DFS, each level represents which candidate number to select, de-duplicate

```python
 if i == idx or candidates[i] != candidates[i - 1]: #handle duplication
```

```python
def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        self.res = []
        candidates.sort(reverse=True)
        self.dfs(0, [], target, candidates)
        return self.res
    
    def dfs(self, idx, path, remain, candidates):
        if remain < 0:
            return
        if remain == 0:
            self.res.append(path)
            return      
        for i in range(idx, len(candidates)):
            if i == idx or candidates[i] != candidates[i - 1]: #handle duplication
                #since one number can only use once, first index is i + 1
                self.dfs(i + 1, path + [candidates[i]], remain - candidates[i], candidates) 
```

## 216. Combination Sum III

Find all possible combinations of **k** numbers that add up to a number **n**, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

* All numbers will be positive integers.
* The solution set must not contain duplicate combinations.

**Example 1:**

```text
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```text
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

### Sol1: DFS: Binary Tree: each level represents select a number or not

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        self.res = []
        self.dfs([], 1, k, n)
        return self.res
    
    def dfs(self, path, idx, k, remain):
        if len(path) == k:
            if remain == 0:
                self.res.append(path)
            return
        #stopping
        if idx > min(9, remain):   
            return
        
        #if select idx
        self.dfs(path + [idx], idx + 1, k, remain - idx)
        #if not select idx
        self.dfs(path, idx + 1, k, remain)
```

## Sol2: each level represents which candidate number to select

```python
 def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        self.res = []
        self.dfs([], 1, k, n)
        return self.res
    
    def dfs(self, path, idx, k, remain):
        if len(path) == k:
            if remain == 0:
                self.res.append(path)
            return
        
        for i in range(idx, 10):
            self.dfs(path + [i], i + 1, k, remain - i)
```

## 377. Combination Sum IV

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

**Example:**

```text
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

**Follow up:**  
What if negative numbers are allowed in the given array?  
How does it change the problem?  
What limitation we need to add to the question to allow negative numbers?

### sol: DP

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        nums.sort()
        dp = [1] + [0] * target
        for i in range(1, target + 1):
            for num in nums:
                if num > i: break
                dp[i] += dp[i - num]
        return dp[-1]
```

This is a 4-line top-down solution that doesn't get accepted due to recursion limit.

```python
class Solution(object):
    def combinationSum4(self, nums, target, memo=collections.defaultdict(int)):
        if target < 0: return 0
        if target not in memo:
            memo[target] += sum((1, self.combinationSum4(nums, target - num))[target != num] 
                                for num in nums)
        return memo[target]
```

The problem with negative numbers is that now the combinations could be potentially of infinite length. Think about `nums = [-1, 1]` and `target = 1`. We can have all sequences of arbitrary length that follow the patterns `-1, 1, -1, 1, ..., -1, 1, 1` and `1, -1, 1, -1, ..., 1, -1, 1` \(there are also others, of course, just to give an example\). So we should limit the _length_ of the combination sequence, so as to give a bound to the problem.

This is a recursive Python code that solves the above follow-up problem, so far it's passed all my test cases but comments are welcome.

```python
import collections
class Solution(object):
    memo = collections.defaultdict(int)
    def combinationSum4WithLength(self, nums, target, length):
        if (target, length) not in self.memo:
            self.memo[target, length] = 0
            if target == 0 and length >= 0:
                self.memo[target, length] += 1
            if length > 0:
                for num in nums:
                    self.memo[target, length] += self.combinationSum4WithLength(nums, target - num, length - 1)
        return self.memo[target, length]
        
#DP
class Solution:
    def combinationSum4WithLength(self, nums: List[int], target: int, length) -> int:
        nums.sort()
        dp = [[0] * (target + 1) for i in range(length))
        dp[0][0] = 1
        for i in range(1, target + 1):
            for j in range(1, length):
                for num in nums:
                    if num > i: break
                    dp[i][j] += dp[i - num][j - 1]
        return sum(dp[-1]) 
```

