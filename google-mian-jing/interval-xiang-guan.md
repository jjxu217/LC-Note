# Interval相关

## 163. Missing Ranges

Given a sorted integer array _**nums**_, where the range of elements are in the **inclusive range** \[lower, upper\], return its missing ranges.

**Example:**

```text
Input: nums = [0, 1, 3, 50, 75], lower = 0 and upper = 99,
Output: ["2", "4->49", "51->74", "76->99"]
```

```python
class Solution:
    def findMissingRanges(self, nums: List[int], lower: int, upper: int) -> List[str]:
        res = []
        pre = lower - 1
        nums.append(upper + 1)
        for n in nums:
            if n > pre + 2:
                res.append(str(pre + 1) + '->' + str(n - 1))
            elif n == pre + 2:
                res.append(str(pre + 1))
            pre = n
        return res 
```

## 程序执行时间， standalone

输入一个list，每个元素包含是一个进程的id和起止时间。 要求输出所有进程的standalone时间（如果有的话）

```text
 input: proc {100, 200, id1}, {150, 300, id2}
 return: {id1, 50} {id2, 100}
```

先按其实时间sort proc；  
Iterate all process， 分类  
1. nxt beginning &gt; cur ending: 记录cur, nxt变cur  
2. nxt ending &gt; cur ending: cur时间=nxt beginning - cur beginning,   
3. nxt ending &gt; cur beginning: 

```python
def CPUStandAlone(CPURunTime):
    if len(CPURunTime) < 1: return []
    res = []
    CPURunTime.sort()
    cur = [cpuRunTime[0][0], cpuRunTime[0][1], cpuRunTime[0][2], 0]
    
    for beg, end, idx in CPURunTime[1:]:
        if cur[1] <= beg:
            res.append((cur[2], cur[1] - cur[0] + cur[3]))
            cur = [beg, end, idx, 0]
        elif cur[1] <= end:
            res.append((cur[2], beg - cur[0]))
            cur = [cur[1], end, idx, 0]
        elif cur[0] <= end:
            cur = [end, cur[1], cur[2], cur[3] + beg - cur[0]]
    res.append((cur[2], cur[3] + cur[1] - cur[0]))        
    return res    
```

