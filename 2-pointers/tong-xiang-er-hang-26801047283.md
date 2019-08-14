# 同向而行 26/80/1047/283

![](../.gitbook/assets/image%20%2817%29.png)

## 26. Remove Duplicates from Sorted Array

Given a sorted array _nums_, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only _once_ and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with O\(1\) extra memory.

Example

```text
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

### idea: slow 左边是处理好的，fast右边是没处理的

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        slow = 1
        for i in range(1, len(nums)):
            if nums[i] != nums[slow - 1]:
                nums[slow] = nums[i]
                slow += 1
        return slow
```

## 80. Remove Duplicates from Sorted Array II

Given a sorted array _nums_, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most _k_ and return the new length.

### Note: !!! compare number at and fast and  slow - k : **nums\[fast\] != nums\[slow - k\]**

```python
class Solution:
    def removeDuplicates(self, k, nums: List[int]) -> int:
        n = len(nums)
        if n < k + 1:
            return n
        slow = k
        for fast in range(k, n):
            if nums[fast] != nums[slow - k]:
                nums[slow] = nums[fast]
                slow += 1
        return slow
```

## Remove Duplicates from Sorted Array变种

对于重复元素，一个都不保留 slow指针：  
all elements to the left hand side of slow\(excluding\) are the final results to return   
begin: head of each distinct elements   
fast: current index

```python
slow = fast = 0
while fast < len(input):
  begin = fast
  while fast < len(input) and input[fast] == input[begin]:
    fast += 1
  if fast - begin == 1:
    input[slow] = input[begin]
    slow += 1
return slow
```

## **1047.** Remove All Adjacent Duplicates In String

Given a string `S` of lowercase letters, a _duplicate removal_ consists of choosing two adjacent and equal letters, and removing them.

We repeatedly make duplicate removals on S until we no longer can.

Return the final string after all such duplicate removals have been made.  It is guaranteed the answer is unique.

**Example 1:**

```text
Input: "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

### Use stack

keep look back, usually use stack

```python
class Solution:
    def removeDuplicates(self, S: str) -> str:
        stack = []
        for i in range(len(S)):
            if not stack or S[i] != stack[-1]:
                stack.append(S[i])
            else:
                stack.pop()
        return ''.join(stack)
```

## 283. Move Zeroes

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```text
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

### Idea

Slow index: All elements to the left hand side of Slow are non zero   
Fast index: explore the right of fast   
set \[Slow, Fast\] = 0

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        slow = 0
        for fast in range(len(nums)):
            if nums[fast] != 0:
                nums[slow] = nums[fast]
                slow += 1
        while slow < len(nums):
            nums[slow] = 0
            slow += 1
```

## 27. Remove Element

Given an array _nums_ and a value _val_, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array** [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with O\(1\) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```text
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for fast in range(len(nums)):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
        return slow
```

## 

