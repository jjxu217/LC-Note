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

