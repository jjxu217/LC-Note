# 406/630 Greedy 2

## 406. Queue Reconstruction by Height

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers `(h, k)`, where `h` is the height of the person and `k` is the number of people in front of this person who have a height greater than or equal to `h`. Write an algorithm to reconstruct the queue.

**Note:**  
The number of people is less than 1,100. 

**Example**

```text
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

### Idea: greedy， 矮子插队无所谓，反正高个看不到: 先排高个，按k排; 再让矮子根据k插队; 矮子插队完全不影响高个的结果

```python
class Solution:
    #greedy， 矮子插队无所谓，反正高个看不到
    #先排高个，按k排; 再让矮子根据k插队; 矮子插队完全不影响高个的结果
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda p: (-p[0], p[1]))
        res = []
        for p in people:
            res.insert(p[1], p)
        return res
```

## 630. Course Schedule III

There are `n` different online courses numbered from `1` to `n`. Each course has some duration\(course length\) `t` and closed on `dth` day. A course should be taken **continuously** for `t` days and must be finished before or on the `dth` day. You will start at the `1st` day.

Given `n` online courses represented by pairs `(t,d)`, your task is to find the maximal number of courses that can be taken.

**Example:**

```text
Input: [[100, 200], [200, 1300], [1000, 1250], [2000, 3200]]
Output: 3
Explanation: 
There're totally 4 courses, but you can take 3 courses at most:
First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day. 
Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day. 
The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.
```

### IDea:

Sort all the courses by ending time. When considering the first K courses, they all end before `end .`Our schedule to be valid, `iff`that \(for all K\), the courses we choose to take within the first K of them, have total `duration` less than `end`.

For each K, we first add the class to schedule, `duration += t`. If `duration` **`> end,`** current schedule is invalid, we will **greedily remove the largest-length course** . To select these largest-length courses, we will use a max heap. `duration`  is the sum of the lengths of the courses we have currently taken.

```python
class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        h = []
        duration = 0
        for t, end in sorted(courses, key=lambda x: x[1]):
            duration += t
            heapq.heappush(h, -t)
            if duration > end:
                duration += heapq.heappop(h)
        return len(h)
```

