# 55/45 Jump Game

## 55. Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

**Example 1:**

```text
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```text
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

```python
class Solution:
#greedy1: time = O(n)
    def canJump(self, nums: List[int]) -> bool:
        n = len(nums)
        lastPos = n - 1
        for i in range(n - 1, -1, -1):
            if i + nums[i] >= lastPos:
                lastPos = i
        return not lastPos
    
#greedy2: Going forwards. m tells the maximum index we can reach so far.
    def canJump(self, nums):
        m = 0
        for i, n in enumerate(nums):
            if i > m:
                return False
            m = max(m, i + n)
        return True
    
#DP solution, O(n ** 2)
    def canJump(self, nums):
        n = len(nums)
        dp = [0] * n
        dp[n - 1] = 1
        for i in range(n - 1, -1, -1):
            j = i + 1
            while j <= i + nums[i] and j < n:
                if dp[j]:
                    dp[i] = 1
                    break
        return dp[0]
```

## 45. Jump Game II

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Example:**

```text
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

```python
class Solution:
    #greedy, O(n)  
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        if n <= 1: return 0
        step, curEnd, nxtEnd = 1, nums[0], nums[0]
        for i in range(1, n):
            if curEnd >= n - 1:
                return step
            nxtEnd = max(nxtEnd, i + nums[i])
            if i == curEnd:
                curEnd = nxtEnd
                step += 1
                
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        curEnd = nxtEnd = step = 0
        for i in range(n - 1):
            nxtEnd = max(nxtEnd, i + nums[i])
            if i == curEnd:
                curEnd = nxtEnd
                step += 1
        return step
    
    
    #dp O(n ** 2)
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [float('inf')] * n
        dp[-1] = 0
        for i in range(n - 1, -1, -1):
            j = i + 1
            while j <= i + nums[i] and j < n:
                dp[i] = min(1 + dp[j], dp[i])
                j += 1
        return dp[0]
                
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        if n <= 1: return 0
        step, curEnd, nxtEnd = 1, nums[0], nums[0]
        for i in range(1, n):
            if curEnd >= n - 1:
                return step
            nxtEnd = max(nxtEnd, i + nums[i])
            if i == curEnd:
                curEnd = nxtEnd
                step += 1
        
```

参考[这里](https://leetcode.wang/leetCode-45-Jump-Game-II.html)，leetCode 讨论里，大部分都是这个思路，贪婪算法，我们每次在可跳范围内选择可以使得跳的更远的位置。

如下图，开始的位置是 2，可跳的范围是橙色的。然后因为 3 可以跳的更远，所以跳到 3 的位置。

![](https://windliang.oss-cn-beijing.aliyuncs.com/45_2.jpg)

如下图，然后现在的位置就是 3 了，能跳的范围是橙色的，然后因为 4 可以跳的更远，所以下次跳到 4 的位置。

![](https://windliang.oss-cn-beijing.aliyuncs.com/45_3.jpg)

写代码的话，我们用 end 表示当前能跳的边界，对于上边第一个图的橙色 1，第二个图中就是橙色的 4，遍历数组的时候，到了边界，我们就重新更新新的边界。

时间复杂度：O（n）。

空间复杂度：O（1）。

这里要注意一个细节，就是 for 循环中，i &lt; nums.length - 1，少了末尾。因为开始的时候边界是第 0 个位置，steps 已经加 1 了。如下图，如果最后一步刚好跳到了末尾，此时 steps 其实不用加 1 了。如果是 i &lt; nums.length，i 遍历到最后的时候，会进入 if 语句中，steps 会多加 1 。

![](https://windliang.oss-cn-beijing.aliyuncs.com/45_4.jpg)

