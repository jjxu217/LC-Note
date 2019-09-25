# 698/416 Partition to K Equal Sum Subsets

## 416. Partition Equal Subset Sum

Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.

**Example 1:**

```text
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```text
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

### Sol: BFS or 0/1 knapsack

```python
class Solution:
    #bfs
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums) % 2 == 0:
            target = sum(nums) // 2
            cur = {0}
            for i in nums:
                cur |= {i + x for x in cur}
                if target in cur:
                    return True
        return False
    
    # 0/1 knapsack 
    def canPartition(self, nums: List[int]) -> bool:
        target, remain = divmod(sum(nums), 2)
        if remain or max(nums) > target: return False

        dp = [False for i in range(target + 1)]
        dp[0] = True
        for num in nums:
            for j in range(target,num-1,-1):
                dp[j] = dp[j] or dp[j - num]
        return dp[target]
```

## 698. Partition to K Equal Sum

Given an array of integers `nums` and a positive integer `k`, find whether it's possible to divide this array into `k` non-empty subsets whose sums are all equal.

**Example 1:**

```text
Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4
Output: True
Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
```

### DFS: back-tracking

```python
class Solution:
    #Time = k * (2^N)
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        target, remain = divmod(sum(nums), k)
        if remain or max(nums) > target: return False
        
        nums.sort(reverse=True) 
        buck = [target] * k
        def dfs(idx):
            if idx == len(nums): 
                return True
            for i in range(k):
                #if buck[i] still has room, add nums[idx], process idx+1
                if buck[i] >=  nums[idx]:
                    buck[i] -=  nums[idx]
                    if dfs(idx + 1): 
                        return True
                    #back-track
                    buck[i] += nums[idx]
#prune: buck[i]==target means putting nums[idx] in this empty bucket can't solve the game, 
#putting nums[idx] on other empty buckets can't solve the game either.
                if buck[i] == target: break  
            return False
        return dfs(0)
```

## 

