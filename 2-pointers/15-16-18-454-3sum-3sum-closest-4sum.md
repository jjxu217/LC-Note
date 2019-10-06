# 对撞类 15/16/259/18/454 3Sum/3Sum Closest Smaller/4Sum

## 15. 3Sum

Given an array `nums` of _n_ integers, are there elements _a_, _b_, _c_ in `nums` such that _a_ + _b_ + _c_ = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```text
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

### Note:

know **de-duplicate**

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        i, n = 0, len(nums)
        for i in range(n - 2):
            if i > 0 and nums[i] == nums[i-1]: #de-duplicate
                continue
            j, k = i + 1, n - 1 #two pointers
            while j < k:
                s = nums[i] + nums[j] + nums[k]
                if s < 0:
                    j += 1
                elif s > 0:
                    k -= 1
                else:
                    res.append([nums[i], nums[j], nums[k]])
                    while j < k and nums[j] == nums[j + 1]: #de-duplicate
                        j += 1
                    while j < k and nums[k] == nums[k - 1]:
                        k -= 1
                    j += 1
                    k -= 1
        return res
```

## 16. 3Sum Closest

Given an array `nums` of _n_ integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```text
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

### Solution:

one pointer + two pointers: Time = O\(n^2\)

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort()
        res = float('inf')
        i, n = 0, len(nums)
        for i in range(n - 2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            j, k = i + 1, n - 1
            while j < k:   
                s = nums[i] + nums[j] + nums[k]
                if abs(s - target) < abs(res - target):
                    res = s
                if s < target:
                    j += 1
                elif s > target:
                    k -= 1
                else:
                    return s            
        return res
```

## 259. 3Sum Smaller

Given an array of n integers nums and a target, find the number of index triplets `i, j, k` with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

**Example:**

```text
Input: nums = [-2,0,1,3], and target = 2
Output: 2 
Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
```

**Follow up:** Could you solve it in O\(n2\) runtime?

**利用 sum &lt; target 时，i 和 left 不动，介于 left 和 right \(包括 right\) 之间的所有元素 sum 也一定小于 target 的单调性。**

```python
    def threeSumSmaller(self, nums: List[int], target: int) -> int:
        nums.sort()
        res = 0
        i, n = 0, len(nums)
        for i in range(n - 2):
            j, k = i + 1, n - 1 #two pointers
            while j < k:
                s = nums[i] + nums[j] + nums[k]
                if s < target:
                    res += k - j
                    j += 1
                else:
                    k -= 1               
        return res
```

## 18. 4Sum

Given an array `nums` of _n_ integers and an integer `target`, are there elements _a_, _b_, _c_, and _d_ in `nums`such that _a_ + _b_ + _c_ + _d_ = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```text
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        res = []
        i, n = 0, len(nums)
        for i in range(n - 3):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            threeResult = self.threeSum(nums[i+1:], target - nums[i])
            for item in threeResult:
                res.append([nums[i]] + item)
        return res
        
    def threeSum(self, nums, target):   
        res = []
        i, n = 0, len(nums)
        for i in range(n - 2):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            j, k = i + 1, n - 1
            while j < k:
                s = nums[i] + nums[j] + nums[k]
                if s < target:
                    j += 1
                elif s > target:
                    k -= 1
                else:
                    res.append([nums[i], nums[j], nums[k]])
                    while j < k and nums[j] == nums[j + 1]:
                        j += 1
                    while j < k and nums[k] == nums[k - 1]:
                        k -= 1
                    j += 1
                    k -= 1
        return res
```

## N-Sum

Recursively reduce N sum until 2 sum

```python
def fourSum(self, nums, target):
    def findNsum(l, r, target, N, result, results):
        if r-l+1 < N or N < 2 or target < nums[l]*N or target > nums[r]*N:  # early termination
            return
        if N == 2: # two pointers solve sorted 2-sum problem
            while l < r:
                s = nums[l] + nums[r]
                if s == target:
                    results.append(result + [nums[l], nums[r]])
                    while l < r and nums[l] == nums[l + 1]:
                        l += 1
                    while l < r and nums[r] == nums[r - 1]:
                        r -= 1
                    l += 1
                    r -= 1
                elif s < target:
                    l += 1
                else:
                    r -= 1
        else: # recursively reduce N
            for i in range(l, r+1):
                if i == l or (i > l and nums[i-1] != nums[i]):
                    findNsum(i+1, r, target-nums[i], N-1, result+[nums[i]], results)

    nums.sort()
    results = []
    findNsum(0, len(nums)-1, target, N, [], results)
    return results
```

## 454. 4Sum II

Given four lists A, B, C, D of integer values, compute how many tuples `(i, j, k, l)` there are such that `A[i] + B[j] + C[k] + D[l]` is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

**Example:**

```text
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

### HashTable: Time=O\(n^2\)

```python
class Solution:
    def fourSumCount(self, A: List[int], B: List[int], C: List[int], D: List[int]) -> int:
        AB = collections.Counter(a+b for a in A for b in B)
        return sum(AB[-c-d] for c in C for d in D)
```

