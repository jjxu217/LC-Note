---
description: FB
---

# 33. Search in Rotated Sorted Array

## 33. Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`\).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array. Your algorithm's runtime complexity must be in the order of _O_\(log _n_\).

**Example 1:**

```text
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```text
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

### Solution1:

Find the minimum value position, then find the target in half of the array.    
The condition and update rule is important. **If right = mid, then left = mid  + 1**

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        #find the minimum value position as mid first
        if not nums:
            return -1
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] < nums[right]:
                right = mid
            else:     #equivalent to nums[mid] >= nums[right], the equal is important
                left = mid + 1  #this left=mid+1 is important, otherwise it might not stop
                      
        #find the target
        left, right = 0, len(nums) - 1
        if  nums[mid] <= target <= nums[right]:
            left = mid
        else:
            right = mid - 1
        while left <= right:
            mid = left + (right - left) // 2
            if target == nums[mid]:
                return mid
            elif target > nums[mid]:
                left = mid + 1
            else:
                right = mid - 1
        return -1
```

### Solution2:

One binary Search.

Note: **nums\[left\] &lt;= target &lt; nums\[mid\], nums\[mid\] &lt; target &lt;= nums\[right\]**, the equation is important.

```python
    def search(self, nums: List[int], target: int) -> int:
        #find the minimum value position as mid first
        if not nums:
            return -1
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if target == nums[mid]:
                return mid
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return -1
```

## 153 Find Minimum in Rotated Sorted Array[  ](https://leetcode.com/explore/learn/card/binary-search/126/template-ii/949/discuss)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`\).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example 1:**

```text
Input: [3,4,5,1,2] 
Output: 1
```

**Example 2:**

```text
Input: [4,5,6,7,0,1,2]
Output: 0
```

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        if not nums:
            return -1
        left, right = 0, len(nums) - 1
        while left < right:
            mid = left + (right - left) // 2
            if nums[mid] < nums[right]:
                right = mid 
            else:                
                left = mid + 1  
        return nums[left]
        
    def findMin(self, nums: List[int]) -> int:
        if not nums:
            return -1
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] < nums[right]:
                right = mid 
            else:  #if  nums[mid] >= nums[right],  '=' to make it break the loop                
                left = mid + 1  
        return nums[mid]
```

## 154. Find Minimum in Rotated Sorted Array II

The array may contain duplicates.

**Example 2:**

```text
Input: [2,2,2,0,1]
Output: 0
```

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        lo, hi = 0, len(nums) - 1
        while lo < hi:
            mid = lo + (hi -lo) // 2
            if nums[mid] > nums[hi]:
                lo = mid + 1
            elif nums[mid] < nums[hi]:
                hi = mid 
            else: #nums[mid] == nums[hi]
                hi -=  1
        return nums[lo]
```

