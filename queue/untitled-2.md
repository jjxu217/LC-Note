# 689. Maximum Sum of 3 Non-Overlapping Subarrays

In a given array `nums` of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size `k`, and we want to maximize the sum of all `3*k` entries.

Return the result as a list of indices representing the starting position of each interval \(0-indexed\). If there are multiple answers, return the lexicographically smallest one.

**Example:**

```text
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```

### Sol: three sliding windows , time=O\(n\), space=O\(1\)

three windows size==k:w1,w2,w3, can just keep 3 adjacent windows move.   
update maxw1 if w1&gt;maxw1;   
update maxw2 if maxw1+w2&gt;maxw2;  
update maxw3 if maxw2+w3&gt;maxw3 

```python
    def maxSumOfThreeSubarrays(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        w1, w2, w3 = sum(nums[:k]), sum(nums[k:2*k]), sum(nums[2*k: 3*k])
        #mw1/2/3 represent the maximum sum with 1/2/3 subarrays so far
        mw1, mw2, mw3=w1,w1+w2,w1+w2+w3
        mw1idx, mw2idx, mw3idx=[0], [0,k], [0,k,2*k]#mw1,mw2,mw3's index.
        
        for i in range(1, n - 3*k + 1):
        #update windows, and mw1/2/3 if needed
            w1 += nums[i-1+k] - nums[i-1]
            w2 += nums[i-1+2*k] - nums[i-1+k]
            w3 += nums[i-1+3*k] - nums[i-1+2*k]
            if w1 > mw1:
                mw1 = w1
                mw1idx = [i]
            if mw1 + w2 > mw2:
                mw2 = mw1 + w2
                mw2idx = [mw1idx[0], i + k]
            if mw2 + w3 > mw3:
                mw3 = mw2 + w3
                mw3idx = [mw2idx[0], mw2idx[1], i + 2 * k]
        return mw3idx
```

