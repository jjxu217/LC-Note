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

```python
class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        intervals.sort(key=lambda x: x[0])
        for i in range(1, len(intervals)):
            if intervals[i][0] < intervals[i-1][1]:
                return False
        return True
```

{% page-ref page="../heap/253.-meeting-rooms-ii.md" %}

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

```python
class Solution:
    def intervalIntersection(self, A: List[List[int]], B: List[List[int]]) -> List[List[int]]:
        res = []
        i = j = 0
        m, n = len(A), len(B)
        while i < m and j < n:
            if A[i][1] < B[j][0]:
                i += 1
            elif A[i][0] > B[j][1]:
                j += 1
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

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

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

