# Smallest subarrays with sum equals K

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
    l1 = l2 = float('inf')
    for i, num in enumerate(nums):
        cur_sum += num
        if cur_sum - K in d:
            if l1 > i - d[cur_sum - K]:
                l2 = l1
                l1 = i - d[cur_sum - K]       
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





{% page-ref page="../array/560.-subarray-sum-equals-k.md" %}

