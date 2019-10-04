# Sort Intervals

## 252. Meeting Rooms

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` \(si &lt; ei\), determine if a person could attend all meetings.

**Example 1:**

```text
Input: [[0,30],[5,10],[15,20]]
Output: false
```

**Example 2:**

```text
Input: [[7,10],[2,4]]
Output: true
```

### Sol: 

先按起始时间, sort interval,  检查相邻2个会的起始时间、终止时间 有无重合。

```python
class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        intervals.sort(key=lambda x: x[0])
        for i in range(1, len(intervals)):
            if intervals[i][0] < intervals[i-1][1]:
                return False
        return True
```

## 253. Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` \(si &lt; ei\), find the minimum number of conference rooms required.

**Example 1:**

```text
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

**Example 2:**

```text
Input: [[7,10],[2,4]]
Output: 1
```

### Solution

**step1: sort the meeting time, according to begin time O\(nlogn\)  
step2:  maintain a MinHeap, according to end time  
           if one meeting end before current meeting begin, free that room, pop the first ele in heap  
           add current ending time to the heap**

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x: x[0])
        #inuse: min-heap record ending time; cnt当前使用的room数
        inuse = []
        res = 0
        for x in intervals:
            #清空已经end的会议
            while inuse and inuse[0] <= x[0]:
                heapq.heappop(inuse)
            heapq.heappush(inuse, x[1])
            res = max(res, len(inuse))
        return res
```

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        if len(intervals) < 2: return len(intervals)
        intervals.sort(key=lambda x: x[0])
        #use a heap to record ending time
        inuse = [intervals[0][1]]
        
        for x in intervals[1:]:
            # If the room due to free up the earliest is free, assign that room to this meeting.
            if inuse[0] <= x[0]:
                heapq.heappop(inuse)
            # If a new room is to be assigned, then also we add to the heap,
            # If an old room is allocated, then also we have to add to the heap with updated end time.
            heapq.heappush(inuse, x[1])           
        return len(inuse)
```

## 56. Merge Intervals

Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```text
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```text
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

### Idea: 

1. **sort interval by beginning time**
2. **case1: 如果新的interval和当前结束时间没有overlapping， 把新的interval加入stack case2: 如果新的interval和当前结束时间有overlapping， ending time是当前结束时间和新的interval时间的较大值**

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])
        stack = []
        for x in intervals:
            if not stack or stack[-1][1] < x[0]:
                stack.append(x)
            else:
                stack[-1][1] = max(stack[-1][1], x[1])
        return stack
```

## 986. Interval List Intersections

Given two lists of **closed** intervals, each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

_\(Formally, a closed interval `[a, b]` \(with `a <= b`\) denotes the set of real numbers `x`with `a <= x <= b`.  The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval.  For example, the intersection of \[1, 3\] and \[2, 4\] is \[2, 3\].\)_

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

```text
Input: A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
Reminder: The inputs and the desired output are lists of Interval objects, and not arrays or lists.
```

### Idea:  2 pointer 指向2 个list

```python
class Solution:
    def intervalIntersection(self, A: List[List[int]], B: List[List[int]]) -> List[List[int]]:
        res = []
        i = j = 0
        m, n = len(A), len(B)
        while i < m and j < n:
            #如果没重叠，往后扫
            if A[i][1] < B[j][0]:
                i += 1
            elif A[i][0] > B[j][1]:
                j += 1
            #如果有重登，res加入重叠部分， 先结束往后扫
            else:                
                res.append([max(A[i][0], B[j][0]), min(A[i][1], B[j][1])])
                if A[i][1] < B[j][1]:
                    i += 1
                else:
                    j += 1
        return res
```

## 57. Insert Interval

Given a set of _non-overlapping_ intervals, insert a new interval into the intervals \(merge if necessary\).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

```text
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```text
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

### Sol: Collect the intervals strictly left or right of the new interval, then merge the new one with the middle ones \(if any\) before inserting it between left and right ones.

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        left, right = [], []
        s, e = newInterval[0], newInterval[1]
        for i in intervals:
            if i[1] < s:
                left.append(i)
            elif i[0] > e:
                right.append(i)
            else:
                s = min(s, i[0])
                e = max(e, i[1])
        return left + [[s, e]] + right
```

