# 349/350/167 Intersection

## 167. Two Sum II - Input array is sorted

Given an array of integers that is already _**sorted in ascending order**_, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

* Your returned answers \(both index1 and index2\) are not zero-based.
* You may assume that each input would have _exactly_ one solution and you may not use the _same_ element twice.

**Example:**

```text
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

### Solution \(two pointers\)

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l, r = 0, len(numbers) - 1
        while l < r:
            s = numbers[l] + numbers[r] 
            if s == target:
                return [l+1, r+1]
            elif s > target:
                r -= 1
            else:
                l += 1
```

### Solution: binary search time = O\(log\(n\)+ log\(n-1\)+...+\(log\(1\)\)\)

```python
 # binary search        
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for i in range(len(numbers)):
            l, r = i + 1, len(numbers) - 1
            tmp = target - numbers[i]
            while l <= r:
                mid = l + (r-l)//2
                if numbers[mid] == tmp:
                    return [i+1, mid+1]
                elif numbers[mid] < tmp:
                    l = mid+1
                else:
                    r = mid-1
```

## 349. Intersection of Two Arrays

Given two arrays, write a function to compute their intersection.

**Example 1:**

```text
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```text
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

**Note:**

* Each element in the result must be unique.
* The result can be in any order.

### Solution1: or two pointer

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        set1 = set(nums1)
        set2 = set(nums2)      
        return list(set(nums1) & set(nums2))
```

### Solution2: Bisect

If the length of one array is smaller than the other. Do bisect

## 350. Intersection of Two Arrays II

Given two arrays, write a function to compute their intersection.

**Example 1:**

```text
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2:**

```text
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

**Note:**

* Each element in the result should appear as many times as it shows in both arrays.
* The result can be in any order.

**Follow up:**

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

```python
class Solution:
#Counter
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        counts = collections.Counter(nums1)
        res = []
        for num in nums2:
            if counts[num] > 0:
                res.append(num)
                counts[num] -= 1
        return res
    
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        a, b = map(collections.Counter, (nums1, nums2))
        return list((a & b).elements())
  
#if sorted, use two pointers  
    def intersect(self, nums1, nums2):
        nums1, nums2 = sorted(nums1), sorted(nums2)
        pt1 = pt2 = 0
        res = []
        while pt1 < len(nums1) and pt2 < len(nums2):
            if nums1[pt1] > nums2[pt2]:
                pt2 += 1
            elif nums1[pt1] < nums2[pt2]:
                pt1 += 1
            else:
                res.append(nums1[pt1])
                pt1 += 1
                pt2 += 1
        return res
```

