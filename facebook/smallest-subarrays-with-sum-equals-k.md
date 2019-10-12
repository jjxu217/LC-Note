# Smallest/non-overlap subarrays with sum equals K

{% page-ref page="../array/560.-subarray-sum-equals-k.md" %}

Given an array `arr` of positive integers only and an integer `k`. Find the minimum length sum of the two shortest subarrays whose sum equals `k`.

**Example 1:**

```text
Input: arr = [5, 1, 2, 2, 3, 4, 1], k = 5
Output: 3
Explanation:
Subarrays whose sum equal 'k' are [5], [1, 2, 2], [2, 3], [4, 1].
The two shortest subarrays are [5], [2, 3]. Hence len([5]) + len([2, 3]) = 1 + 2 = 3.
[5], [4, 1] also works but that does not change the final answer.
```

#### dict{prefix sum: idx} 

```python
# time: O(n ), space: O(n)
def min_intervals_length_sum(nums, K):
    n = len(nums)
    d = {0 : -1} # {prefix sum: index}
    cur_sum = 0
    l1 = l2 = float('inf') #two shortest subarray length
    for i, num in enumerate(nums):
        cur_sum += num
        if cur_sum - K in d:
            #更新最小
            if l1 > i - d[cur_sum - K]:
                l2 = l1
                l1 = i - d[cur_sum - K]       
            #更新第二小
            elif l2 > i - d[cur_sum - K]:
                l2 = i - d[cur_sum - K]
        d[cur_sum] = i
    return l1 + l2

A=[5,1,2,2,3,4,1]
K=5
print(min_intervals_length_sum(A, K))
```

**Follow-up:**  
What if the array has negative numbers?

## Non-overlap subarray sum to k

给一个数组（只有正整数），给一个数K，找出两段不overlap的subarray使得每一段interval里数的和为K，求两段interval长度之和的最小值。 题目有点绕，e.g. A=\[1,2,2,3,1,4\], K=5, 能找到的interval是 \[1,2,2\] & \[1,4\] \[2,3\] & \[1,4\] 返回 4 因为\[2,3\], \[1,4\]是和最小的两段，而不是\[1,2,2\]，\[1,4\]

**求前缀和， 先从前往后一遍求i \(include\)左边最短的subarray sum to K的长度`left[i];`   
再求后缀和，从后往前一遍求i \(include\)右边最短的subarray sum to K的长度`right[i],` 翻转再翻转；  
最后对每个i算左右总数最小的**

```python
class Solution(object):
    def minLeftSub(self, arr, K):
        n = len(arr)
        preSum = 0
        dic = {0, -1}
        
        minleft = []
        cur_min = float('inf')
        for i in range(n):
            preSum += arr[i]
            if preSum - K in dic:
                cur_min = min(cur_min, i - dic[preSum - K])
            minleft.append(cur_min)               
            dic[preSum] = i
        return minleft
    
    def minSubarray(self, arr, K):
        #left[i] 表示i(include)左边的 valid subarray 的最小length
        #right[i] 表示i(include)右边的 valid subarray的最小length
        left = minLeftSub(arr, K)
        right = minLeftSub(arr[::-1], K)[::-1]
        
        res = min(left[i] + right[i + 1] for i in range(n - 1))         
        return res if res != float('inf') else -1
```



{% page-ref page="../array/560.-subarray-sum-equals-k.md" %}

