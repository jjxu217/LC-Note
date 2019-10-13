# 217/219/220 Contains Duplicate

## 217. Contains Duplicate

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

**Example 1:**

```text
Input: [1,2,3,1]
Output: true
```

**Example 2:**

```text
Input: [1,2,3,4]
Output: false
```

### Solution1: Sort\(\)  

### Solution2: HashSet

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        s = set()
        for i in nums:
            if i in s:
                return True
            s.add(i)
        return False
```

## 219. Contains Duplicate II

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that **nums\[i\] = nums\[j\]** and the **absolute** difference between i and j is at most k.

**Example 1:**

```text
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```text
Input: nums = [1,0,1,1], k = 1
Output: true
```

### Solution: HashSet

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        if len(nums) < 2 or k < 1:
            return False
        s = set()
        for i in range(len(nums)):
            if nums[i] in s:
                return True
            s.add(nums[i])
            if len(s) > k:
                s.remove(nums[i - k])      
        return False
```

## 220. Contains Duplicate III

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the **absolute** difference between **nums\[i\]** and **nums\[j\]** is at most t and the **absolute** difference between i and j is at most k.

**Example 1:**

```text
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```text
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

### Solution: BucketSort\(\)， 

Maintain at most k buckets with width of \(t+1\). bucket = {idx: num}  
If there are two item with difference &lt;= t, either the two in the same bucket or the two in neighbor buckets

```python
class Solution: 
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        if t < 0: return False
        bucket = {}
        #the width of the bucket
        w = t + 1
        for i in range(len(nums)):
            #bucket place
            n = nums[i] // w
            #if current bucket contains element
            if n in bucket:
                return True
            #if neigh buckets contains element and the difference within t
            for nb in (n - 1, n + 1):
                if nb in bucket and abs(bucket[nb] - nums[i]) <= t:
                    return True
            bucket[n] = nums[i]
            #remain at most k buckets 
            if len(bucket) > k:
                del bucket[nums[i - k] // w]
        return False
```

