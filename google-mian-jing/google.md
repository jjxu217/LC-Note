# Google

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

## 818. Race Car

Your car starts at position 0 and speed +1 on an infinite number line.  \(Your car can go into negative positions.\)

Your car drives automatically according to a sequence of instructions A \(accelerate\) and R \(reverse\).

When you get an instruction "A", your car does the following: `position += speed, speed *= 2`.

When you get an instruction "R", your car does the following: if your speed is positive then `speed = -1` , otherwise `speed = 1`.  \(Your position stays the same.\)

For example, after commands "AAR", your car goes to positions 0-&gt;1-&gt;3-&gt;3, and your speed goes to 1-&gt;2-&gt;4-&gt;-1.

Now for some target position, say the **length** of the shortest sequence of instructions to get there.

```text
Example 1:
Input: 
target = 3
Output: 2
Explanation: 
The shortest instruction sequence is "AA".
Your position goes from 0->1->3.
```

```text
Example 2:
Input: 
target = 6
Output: 5
Explanation: 
The shortest instruction sequence is "AAARA".
Your position goes from 0->1->3->7->7->6.
```

### Idea: DP

For the input 5, we can reach with only 7 steps: AARARAA. Because we can step back.  
Let's say `n` is the length of `target` in binary and we have `2 ^ (n - 1) <= target < 2 ^ n`  
We have 2 strategies here  
**1. Go pass our target , stop and turn back**  
We take `n` instructions of `A`.  
`1 + 2 + 4 + ... + 2 ^ (n-1) = 2 ^ n - 1`  
Then we turn back by one `R` instruction.  
In the end, we get closer by `n + 1` instructions.  
**2. Go as far as possible before pass target, stop and turn back**  
We take `N - 1` instruction of `A` and one `R`.  
Then we take `m` instructions of `A`, where `m < n`

#### Consider two general cases for number `i` with bit\_length `n`.

1. `i==2^n-1`, this case, `n` is the best way
2. `2^(n-1)-1<i<2^n-1`, there are two ways to arrive at `i`:
   * Use `n` A to arrive at `2^n-1` first, then use R to change the direction, by here there are `n+1` operations \(n A and one R\), then the remaining is same as `dp[2^n-1-i]`
   * Use `n-1` A to arrive at `2^(n-1)-1` first, then R to change the direction, use `m` A to go backward, then use `R` to change the direction again to move forward, by here there are `n-1+2+m=n+m+1` operations \(`n-1` A, two R, `m` A\) , current position is `2^(n-1)-1-(2^m-1)=2^(n-1)-2^m`, the remaining operations is same as `dp[i-(2^(n-1)-1)+(2^m-1)]=dp[i-2^(n-1)+2^m)]`.

```python
class Solution:
    def __init__(self): 
        self.dp = {0: 0}

    def racecar(self, t):
        if t in self.dp: 
            return self.dp[t]
        n = t.bit_length()
        #case 1: t == 2 ^ n-1
        if 2 ** n - 1 == t: 
            self.dp[t] = n
        #case 2: 2 ^ (n-1) - 1 < i < 2 ^ n - 1
        else:
            #method1: Use n A to arrive at 2 ^ n - 1 first, then use R to change the direction
            self.dp[t] = self.racecar(2**n - 1 - t) + n + 1
            #method2: Use n-1 A to arrive at 2 ^ (n-1)-1 first, then R to change the direction, use m A to go backward, then use R to change the direction again to move forward,
            for m in range(n - 1):
                self.dp[t] = min(self.dp[t], self.racecar(t - 2 ** (n - 1) + 2 ** m) + n + m + 1)
        return self.dp[t]
```

## 1143. Longest Common Subsequence

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A _subsequence_ of a string is a new string generated from the original string with some characters\(can be none\) deleted without changing the relative order of the remaining characters. \(eg, "ace" is a subsequence of "abcde" while "aec" is not\). A _common subsequence_ of two strings is a subsequence that is common to both strings.

If there is no common subsequence, return 0.

**Example 1:**

```text
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```text
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```text
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

### DP

```python
class Solution:
    #2d DP
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:     
        dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
        for i, c in enumerate(text1):
            for j, d in enumerate(text2):
                dp[i + 1][j + 1] = 1 + dp[i][j] if c == d else max(dp[i][j + 1], dp[i + 1][j])
        return dp[-1][-1]
    
    #滚动数组
    #use 1 - i % 2 and i % 2 to switch between dp[0] (row 0) and dp[1] (row 1)
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        if m < n:
            return self.longestCommonSubsequence(text2, text1) 
        dp = [[0] * (n + 1) for _ in range(2)]
        for i, c in enumerate(text1):
            for j, d in enumerate(text2):
                dp[1 - i % 2][j + 1] = 1 + dp[i % 2][j] if c == d else max(dp[i % 2][j + 1], dp[1 - i % 2][j])
        return dp[m % 2][-1]
```

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

