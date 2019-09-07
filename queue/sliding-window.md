# Sliding window

## 76. Minimum Window Substring

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O\(n\).

**Example:**

```text
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

**Note:**

* If there is no such window in S that covers all characters in T, return the empty string `""`.
* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        need = collections.Counter(t)  #hashtable to store char frequency, 
        missing = len(t)        #total number of chars we need
        start = end = i = 0 
        for j, char in enumerate(s, 1): #index j from 1
            if need[char] > 0:
                missing -= 1
            need[char] -= 1
            if missing == 0:             #match all chars
                while i < j and need[s[i]] < 0:     #remove chars to find the real start
                    need[s[i]] += 1
                    i += 1                
                if end == 0 or j - i < end - start: #update window
                    start, end = i, j
                #left point +1, remove the first needed char in the previous windows
                missing = 1
                need[s[i]] = 1                              
                i += 1                
        return s[start:end]    
```

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

### Solution: use a descending deque

The algorithm is quite straightforward :

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

### Sol1: maintain a sorted array length of k, keep remove first one and add new one\(with bisect.insort\), time = O\(nk\)

```python
class Solution:
    def medianSlidingWindow(self, nums: List[int], k: int) -> List[float]:
        window = sorted(nums[:k])
        medians = [(window[k//2] + window[~(k//2)]) / 2]
        for a, b in zip(nums, nums[k:]):
            window.remove(a)
            bisect.insort(window, b)
            medians.append((window[k//2] + window[~(k//2)]) / 2)
        return medians
```

### Sol: min-heap + max-heap, time=O\(nlogk\)

#### 2-heap \(min, max\) based solution:

* uses 2 heaps left-\(max\)heap `lh` and right-\(min\)heap `rh`. The key idea is the maintain the size invariance of the heaps as we add and remove elements. The top of both the heaps can be used to calculate the median.
* We use `lazy-deletion` from the heap
* using the first `k` elements construct a min heap `lh`. Then pop `k-k/2` and add it to the `rh`. Now the heap sized are set at `k/2` and `k-k/2`
* Iterate over rest of the numbers and add it to the appropriate heap and maintain heap size invariance by moving the top number from one heap to another as needed.

```python
    def medianSlidingWindow(self, nums, k): 
        lh,rh,medians = [],[],[]
        # create the initial left and right heap
        for i,n in enumerate(nums[:k]): 
            heappush(lh, (-n,i))
        for i in range(k-k//2):
            heappush(rh, (-lh[0][0], lh[0][1]))
            heappop(lh)
        medians.append(rh[0][0] if k%2 else (rh[0][0] - lh[0][0])/2)
        
        for i,n in enumerate(nums[k:]):          
            if n >= rh[0][0]:
                heappush(rh,(n,i+k))        # rh +1
                if nums[i] <= rh[0][0]:     # lh-1, unbalanced
                    heappush(lh, (-rh[0][0], rh[0][1]))
                    heappop(rh)
                # else: pass                # rh-1, balanced
            else:
                heappush(lh,(-n,i+k))        # rh +1
                if nums[i] >= rh[0][0]:     # rh-1, unbalanced
                    heappush(rh, (-lh[0][0], lh[0][1]))
                    heappop(lh)
                # else: pass                # lh-1, balanced
            while(lh and lh[0][1] <= i): heappop(lh)  # lazy-deletion
            while(rh and rh[0][1] <= i): heappop(rh)  # lazy-deletion
            medians.append(rh[0][0] if k%2 else (rh[0][0] - lh[0][0])/2)
        return medians
```

## 295. Find Median from Data Stream

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.For example,

`[2,3,4]`, the median is `3`

`[2,3]`, the median is `(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

* void addNum\(int num\) - Add a integer number from the data stream to the data structure.
* double findMedian\(\) - Return the median of all elements so far.

**Example:**

```text
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

**Follow up:**

1. If all integer numbers from the stream are between 0 and 100, how would you optimize it?
2. If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?

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

