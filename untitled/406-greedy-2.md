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

## 1007. Minimum Domino Rotations For Equal Row

In a row of dominoes, `A[i]` and `B[i]` represent the top and bottom halves of the `i`-th domino.  \(A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.\)

We may rotate the `i`-th domino, so that `A[i]` and `B[i]` swap values.

Return the minimum number of rotations so that all the values in `A` are the same, or all the values in `B` are the same.

If it cannot be done, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/03/08/domino.png)

```text
Input: A = [2,1,2,4,2,2], B = [5,2,6,2,3,2]
Output: 2
Explanation: 
The first figure represents the dominoes as given by A and B: before we do any rotations.
If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.
```

**Example 2:**

```text
Input: A = [3,5,1,2,3], B = [3,6,3,3,4]
Output: -1
Explanation: 
In this case, it is not possible to rotate the dominoes to make one row of values equal.
```

```python
class Solution:
       # One observation is that, if A[0] works, no need to check B[0].
       # Because if both A[0] and B[0] exist in all dominoes,
       # the result will be the same.
    def minDominoRotations(self, A: List[int], B: List[int]) -> int:
        def check(x):
            # how many rotations should be done
            # to have all elements in A/B equal to x
            rotations_a = rotations_b = 0
            for i in range(n):
                if A[i] != x and B[i] != x:
                    return -1
                elif A[i] != x:
                    rotations_a += 1
                elif B[i] != x:
            return min(rotations_a, rotations_b)
    
        n = len(A)
        rotations = check(A[0]) 
        # If one could make all elements in A or B equal to A[0]
        if rotations != -1 or A[0] == B[0]:
            return rotations 
        # If one could make all elements in A or B equal to B[0]
        else:
            return check(B[0])
```

