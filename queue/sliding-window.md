# Sliding window

## 239. Sliding Window Maximum

Given an array _nums_, there is a sliding window of size _k_ which is moving from the very left of the array to the very right. You can only see the _k_ numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

**Example:**

```text
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Note:**  
You may assume _k_ is always valid, 1 ≤ k ≤ input array's size for non-empty array.

**Follow up:**  
Could you solve it in linear time?

### Solution: use a decending deque

The algorithm is quite straigthforward :

* Iterate over the array. At each step :
  * Clean the deque :
    * Keep only the indexes of elements from the current sliding window.
    * Remove indexes of all elements smaller than the current one, since they will not be the maximum ones.
  * Append the current element to the deque.
  * If i &gt;= k - 1, append `deque[0]` to the output.
* Return the output array.

```python
class Solution:   
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        d = collections.deque()
        res = []
        for i, n in enumerate(nums):
            #clean the deque: remove index outside sliding windows, remove smaller element
            if d[0] == i - k:
                d.popleft()
            while d and n > nums[d[-1]]:
                d.pop()
                
            d.append(i)  
            if i >= k - 1:
                res.append(nums[d[0]])
        return res
```

## 480. Sliding Window Median

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.Examples:  


`[2,3,4]` , the median is `3`

`[2,3]`, the median is `(2 + 3) / 2 = 2.5`

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,  
Given nums = `[1,3,-1,-3,5,3,6,7]`, and k = 3.

```text
Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

Therefore, return the median sliding window as `[1,-1,-1,3,5,6]`.

**Note:**  
You may assume `k` is always valid, ie: `k` is always smaller than input array's size for non-empty array.

```text

```

## 360. Moving Average from Data Stream

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

**Example:**

```text
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

```python
class MovingAverage:
    def __init__(self, size: int):
        self.l = []
        self.size = size
        self.cur = 0
        self.sum = 0

    def next(self, val: int) -> float:
        if len(self.l) < self.size:
            self.l.append(val)
            self.sum += val
        else:
            pre = self.l[self.cur]
            self.l[self.cur] = val
            self.sum +=  val - pre
        self.cur = (self.cur + 1) % self.size  
        return self.sum / len(self.l)
    
   #use deque 
    def __init__(self, size):
        self.queue = collections.deque(maxlen=size)
        self.wsize = size
        self.cursum = 0
        
    def next(self, val):
        self.cursum += val
        if len(self.queue) == self.wsize:
            self.cursum -= self.queue.popleft()
        self.queue.append(val)
        return self.cursum / len(self.queue)
```

