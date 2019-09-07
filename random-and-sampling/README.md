# Random & Sampling

## 470. Implement Rand10\(\) Using Rand7\(\)

Given a function `rand7` which generates a uniform random integer in the range 1 to 7, write a function `rand10` which generates a uniform random integer in the range 1 to 10.

Do NOT use system's `Math.random()`.

**Example 1:**

```text
Input: 1
Output: [7]
```

**Example 3:**

```text
Input: 3
Output: [8,1,10]
```

```python
class Solution:
    def rand10(self):
        while True:
            num = 7 * (rand7() - 1) + (rand7() - 1)
            if num < 40:
                return num % 10 + 1
```

## 478. Generate Random Point in a Circle

Given the radius and x-y positions of the center of a circle, write a function `randPoint` which generates a uniform random point in the circle.

Note:

1. input and output values are in [floating-point](https://www.webopedia.com/TERM/F/floating_point_number.html).
2. radius and x-y position of the center of the circle is passed into the class constructor.
3. a point on the circumference of the circle is considered to be in the circle.
4. `randPoint` returns a size 2 array containing x-position and y-position of the random point, in that order.

**Example 1:**

```text
Input: 
["Solution","randPoint","randPoint","randPoint"]
[[1,0,0],[],[],[]]
Output: [null,[-0.72939,-0.65505],[-0.78502,-0.28626],[-0.83119,-0.19803]]
```

**Example 2:**

```text
Input: 
["Solution","randPoint","randPoint","randPoint"]
[[10,5,-7.5],[],[],[]]
Output: [null,[11.52438,-8.33273],[2.46992,-16.21705],[11.13430,-12.42337]]
```

```python
import random

class Solution:
    def __init__(self, radius: float, x_center: float, y_center: float):
        self.center = [x_center, y_center]
        self.radius = radius

    #Rejection Sampling
    def randPoint(self) -> List[float]:     
        while True:
            x, y = random.uniform(-1, 1), random.uniform(-1, 1)
            if x ** 2 + y ** 2 <= 1:
                return [self.center[0] + self.radius * x, self.center[1] + self.radius * y]
            
    #radius sampling        
    def randPoint(self) -> List[float]:
        r = self.radius * math.sqrt(random.uniform(0, 1))
        degree = 2 * math.pi * random.uniform(0, 1)
        return [self.center[0] + r * math.cos(degree), self.center[1] + r * math.sin(degree)]
        
# Your Solution object will be instantiated and called as such:
# obj = Solution(radius, x_center, y_center)
# param_1 = obj.randPoint()
```

## 710. Random Pick with Blacklist

Given a blacklist `B` containing unique integers from `[0, N)`, write a function to return a uniform random integer from `[0, N)` which is **NOT** in `B`.

Optimize it such that it minimizes the call to system’s `Math.random()`.

**Note:**

1. `1 <= N <= 1000000000`
2. `0 <= B.length < min(100000, N)`
3. `[0, N)` does NOT include N. See [interval notation](https://en.wikipedia.org/wiki/Interval_%28mathematics%29).

**Example 1:**

```text
Input: 
["Solution","pick","pick","pick"]
[[1,[]],[],[],[]]
Output: [null,0,0,0]
```

**Example 2:**

```text
Input: 
["Solution","pick","pick","pick"]
[[2,[]],[],[],[]]
Output: [null,1,1,1]
```

### Sol: Re-mapping

Treat the first N - \|B\| numbers as those we can pick from. Iterate through the blacklisted numbers and map each of them to to one of the remaining non-blacklisted \|B\| numbers

For picking, just pick a random uniform int in 0, N - \|B\|. If it's not blacklisted, return the number. If it is, return the number that its mapped to

```python
class Solution:
    def __init__(self, N: int, blacklist: List[int]):
        self.m = {}
        self.L = N - len(blacklist)
        blackset = set(blacklist)     
        blacklist.sort()
        j = 0
        for i in range(self.L, N):
            if i not in blackset:
                self.m[blacklist[j]] = i
                j += 1

    def pick(self) -> int:
        u = random.randrange(self.L)
        return self.m.get(u, u)
```

## 497. Random Point in Non-overlapping Rectangles

Given a list of **non-overlapping** axis-aligned rectangles `rects`, write a function `pick` which randomly and uniformily picks an **integer point** in the space covered by the rectangles.

Note:

1. An **integer point** is a point that has integer coordinates. 
2. A point on the perimeter of a rectangle is **included** in the space covered by the rectangles. 
3. `i`th rectangle = `rects[i]` = `[x1,y1,x2,y2]`, where `[x1, y1]` are the integer coordinates of the bottom-left corner, and `[x2, y2]` are the integer coordinates of the top-right corner.
4. length and width of each rectangle does not exceed `2000`.
5. `1 <= rects.length <= 100`
6. `pick` return a point as an array of integer coordinates `[p_x, p_y]`
7. `pick` is called at most `10000` times.

**Example 1:**

```text
Input: 
["Solution","pick","pick","pick"]
[[[[1,1,5,5]]],[],[],[]]
Output: 
[null,[4,1],[4,1],[3,3]]
```

**Example 2:**

```text
Input: 
["Solution","pick","pick","pick","pick","pick"]
[[[[-2,-2,-1,-1],[1,0,3,0]]],[],[],[],[],[]]
Output: 
[null,[-1,-2],[2,0],[-2,-1],[3,0],[-2,-2]]
```

### Sol: prefix-sum + Bisect

Must use **bisect.bisect**, choose the right element.

```python
class Solution:
    def __init__(self, rects: List[List[int]]):
        self.rects, self.ranges, sm = rects, [0], 0
        for x1,y1,x2,y2 in rects:
            sm += (x2 - x1 + 1) * (y2 - y1 + 1)
            self.ranges.append(sm)

    def pick(self) -> List[int]:
        n = random.randrange(self.ranges[-1])
        i = bisect.bisect(self.ranges, n)
        n -= self.ranges[i - 1]
        x1,y1,x2,y2 = self.rects[i - 1]
        return [x1 + n % (x2 - x1 + 1), y1 + n // (x2 - x1 + 1)]
```

## 519. Random Flip Matrix

You are given the number of rows `n_rows` and number of columns `n_cols` of a 2D binary matrix where all values are initially 0. Write a function `flip` which chooses a 0 value [uniformly at random](https://en.wikipedia.org/wiki/Discrete_uniform_distribution), changes it to 1, and then returns the position `[row.id, col.id]` of that value. Also, write a function `reset` which sets all values back to 0. **Try to minimize the number of calls to system's Math.random\(\)** and optimize the time and space complexity.

Note:

1. `1 <= n_rows, n_cols <= 10000`
2. `0 <= row.id < n_rows` and `0 <= col.id < n_cols`
3. `flip` will not be called when the matrix has no 0 values left.
4. the total number of calls to `flip` and `reset` will not exceed 1000.

**Example 1:**

```text
Input: 
["Solution","flip","flip","flip","flip"]
[[2,3],[],[],[],[]]
Output: [null,[0,1],[1,2],[1,0],[1,1]]
```

**Example 2:**

```text
Input: 
["Solution","flip","flip","reset","flip"]
[[1,2],[],[],[],[]]
Output: [null,[0,0],[0,1],null,[0,0]]
```

### Solution: Shuffling

Here we are modeling the matrix as a 1d array with initial size of row\*cols.  
For each flip, randomly pick an index in \[0, tail\] and flip it.   
**rand = random.randint\(0, self.tail**\);   
res is _rand_ or if rand is swapped, is the swapped number _d\[rand\]_ : **res = self.d.get\(rand, rand\)**   
Then swap that flipped element with the tail element, store that mapping info \(key: origin index, value: index of the tail\) into a Map and decrease the size. **self.d\[rand\] = self.d.get\(self.tail, self.tail\)**;   
Next time when we randomly pick a same index we can go to the map and find the actual element stores in that index

```python
class Solution:
        self.c = n_cols
        self.r = n_rows
        self.tail = n_rows * n_cols - 1
        self.d = {}
        
    def flip(self):
        rand = random.randint(0, self.tail)
        res = self.d.get(rand, rand)
        self.d[rand] = self.d.get(self.tail, self.tail)
        self.tail -= 1       
        return divmod(res, self.c)
    
    def reset(self):
        self.d = {}
        self.tail = self.c * self.r - 1
```

