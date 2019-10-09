# Array/Matrix

## 841. Keys and Rooms

There are `N` rooms and you start in room `0`.  Each room has a distinct number in `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room. 

Formally, each room `i` has a list of keys `rooms[i]`, and each key `rooms[i][j]` is an integer in `[0, 1, ..., N-1]` where `N = rooms.length`.  A key `rooms[i][j] = v` opens the room with number `v`.

Initially, all the rooms start locked \(except for room `0`\). You can walk back and forth between rooms freely.Return `true` if and only if you can enter every room.

**Example 1:**

```text
Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.
```

**Example 2:**

```text
Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
```

```python
class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        n = len(rooms)
        enter = [False] * n
        stack = [0]
        while stack:
            cur = stack.pop()
            if not enter[cur]:
                enter[cur] = True
                stack += rooms[cur]
        return all(enter)           
```

## 911. Online Election

In an election, the `i`-th vote was cast for `persons[i]` at time `times[i]`.

Now, we would like to implement the following query function: `TopVotedCandidate.q(int t)` will return the number of the person that was leading the election at time `t`.  

Votes cast at time `t` will count towards our query.  In the case of a tie, the most recent vote \(among tied candidates\) wins.

**Example 1:**

```text
Input: ["TopVotedCandidate","q","q","q","q","q","q"], [[[0,1,1,0,0,1,0],[0,5,10,15,20,25,30]],[3],[12],[25],[15],[24],[8]]
Output: [null,0,1,1,0,0,1]
Explanation: 
At time 3, the votes are [0], and 0 is leading.
At time 12, the votes are [0,1,1], and 1 is leading.
At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.)
This continues for 3 more queries at time 15, 24, and 8.
```

### **Use a list has the same length as time,** self.leads\[i\] represent the lead at times\[i\]

Initialization part:  
In the order of time, we count the number of votes for each person.  
Also, we update the current lead of votes for each time point.  
`if (count[person] >= count[lead]) lead = person`  
Time Complexity: `O(N)`

Query part:  
Binary search `t` in `times`, find out the latest time point no later than `t`.  
Return the lead of votes at that time point.  
Time Complexity: `O(logN)`

```python
class TopVotedCandidate:  
    def __init__(self, persons, times):
        #self.leads[i] represent the lead at times[i]
        self.leads, count, cur_lead, self.times = [], collections.Counter(), -1, times
        for p in persons:
            count[p] += 1
            if count[p] >= count[cur_lead]: 
                cur_lead = p
            self.leads.append(cur_lead)

    def q(self, t):
        return self.leads[bisect.bisect_right(self.times, t) - 1]
```

## 853. Car Fleet

`N` cars are going to the same destination along a one lane road.  The destination is `target` miles away.

Each car `i` has a constant speed `speed[i]` \(in miles per hour\), and initial position `position[i]` miles towards the target along the road.

A car can never pass another car ahead of it, but it can catch up to it, and drive bumper to bumper at the same speed.The distance between these two cars is ignored - they are assumed to have the same position.

A _car fleet_ is some non-empty set of cars driving at the same position and same speed.  Note that a single car is also a car fleet.

If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.  
How many car fleets will arrive at the destination?

**Example 1:**

```text
Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
Output: 3
Explanation:
The cars starting at 10 and 8 become a fleet, meeting each other at 12.
The car starting at 0 doesn't catch up to any other car, so it is a fleet by itself.
The cars starting at 5 and 3 become a fleet, meeting each other at 6.
Note that no other cars meet these fleets before the destination, so the answer is 3.
```

### Idea: 

**Sort** cars by the start positions `pos`  
Loop on each car from end to beginning  
Calculate its `time` to arrive the `target；cur` records the current biggest time \(the slowest\)

If another car needs  `>cur`, it will be the new slowest car, and becomes the new lead of a car fleet.   
Else, it can catch up this car fleet.

```python
def carFleet(self, target, pos, speed):
        time = [(target - p) / s for p, s in sorted(zip(pos, speed))]
        res = cur = 0
        for t in time[::-1]:
            if t > cur:
                res += 1
                cur = t
        return res
```

## 750. Number Of Corner Rectangles

Given a grid where each entry is only 0 or 1, find the number of corner rectangles.

A _corner rectangle_ is 4 distinct 1s on the grid that form an axis-aligned rectangle. Note that only the corners need to have the value 1. Also, all four 1s used must be distinct.

**Example 1:**

```text
Input: grid = 
[[1, 0, 0, 1, 0],
 [0, 0, 1, 0, 1],
 [0, 0, 0, 1, 0],
 [1, 0, 1, 0, 1]]
Output: 1
Explanation: There is only one corner rectangle, with corners grid[1][2], grid[1][4], grid[3][2], grid[3][4].
```

**Example 2:**

```text
Input: grid = 
[[1, 1, 1],
 [1, 1, 1],
 [1, 1, 1]]
Output: 9
Explanation: There are four 2x2 rectangles, four 2x3 and 3x2 rectangles, and one 3x3 rectangle.
```

**Example 3:**

```text
Input: grid = 
[[1, 1, 1, 1]]
Output: 0
Explanation: Rectangles must have four distinct corners.
```

### Sol: count the number of 1 in both column j/k, Counter\[k,j\]

```python
class Solution:
    def countCornerRectangles(self, grid: List[List[int]]) -> int:
        counts, res = collections.Counter(), 0
        for i, row in enumerate(grid):
            for j, val in enumerate(row):
                if val:
                    for k in range(j):
                        if grid[i][k]:
                            res += counts[(k, j)]
                            counts[(k, j)] += 1
        return res
```



