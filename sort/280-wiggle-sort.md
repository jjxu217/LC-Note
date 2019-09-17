# 280/324 Wiggle Sort

## 280. Wiggle Sort

Given an unsorted array `nums`, reorder it **in-place** such that `nums[0] <= nums[1] >= nums[2] <= nums[3]...`.

**Example:**

```text
Input: nums = [3,5,2,1,6,4]
Output: One possible answer is [3,5,1,6,2,4]
```

### Solution: one pass

As we iterate through the array, we compare the current element to its next element and if the order is incorrect, we swap them. In-place

```python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        for i in range(len(nums) - 1):
            if (i % 2 == 0 and nums[i] > nums[i + 1]) or (i % 2  and nums[i] < nums[i + 1]):
                nums[i], nums[i + 1] = nums[i + 1], nums[i]
```

## 324. Wiggle Sort II

Given an unsorted array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]...`.

**Example 1:**

```text
Input: nums = [1, 5, 1, 1, 6, 4]
Output: One possible answer is [1, 4, 1, 5, 1, 6].
```

**Example 2:**

```text
Input: nums = [1, 3, 2, 2, 3, 1]
Output: One possible answer is [2, 3, 1, 3, 1, 2].
```

**Follow Up:**  
Can you do it in O\(n\) time and/or in-place with O\(1\) extra space?

### Idea: 

把数分成S/L 两个group, 偶数位放小的数，奇数位放大的数。把中间可能相同的数错开。

1. sort the list
2. 偶数位是前一半较小数从大到小排，奇数位是后一半较大数从大到小排

```python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        nums.sort()
        half = len(nums[::2])
        nums[::2], nums[1::2] = nums[:half][::-1], nums[half:][::-1]
```

{% embed url="https://leetcode.com/problems/wiggle-sort-ii/discuss/77684/Summary-of-the-various-solutions-to-Wiggle-Sort-for-your-reference" %}



