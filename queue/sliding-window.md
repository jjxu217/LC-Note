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
        need = collections.Counter(t)  #hashtable to store char frequency
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
                i += 1     
                missing = 1
                need[s[i]] = 1                                               
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

### Solution: use a descending deque, record the idx

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
            #clean the deque: remove index outside sliding windows, 
            # remove smaller element from the right side
            if d and d[0] == i - k:
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
class Solution:
    def medianSlidingWindow(self, nums, k):
        def move(h1, h2): #pop one ele from h1 and push to h2
            x, i = heapq.heappop(h1)
            heapq.heappush(h2, (-x, i))
        
        small, large = [], []
        for i, n in enumerate(nums[:k]): 
            heapq.heappush(small, (-n,i))
        for _ in range(k - k // 2): 
            move(small, large)
        ans = [large[0][0] if k % 2 else (large[0][0] - small[0][0])/2]
        
        for i, n in enumerate(nums[k:]):
            if n >= large[0][0]:
                heapq.heappush(large, (n, i+k)) # large + 1
                if nums[i] <= large[0][0]:  # small - 1, unbalanced
                    move(large, small)
                #else: pass  # large - 1, balanced
            else:
                heapq.heappush(small, (-n, i+k)) #small + 1
                if nums[i] >= large[0][0]: #large - 1, unbalanced
                    move(small, large)
                #else: pass  # small - 1, balanced
            while small and small[0][1] <= i: heapq.heappop(small) # lazy-deletion
            while large and large[0][1] <= i: heapq.heappop(large)
            ans.append(large[0][0] if k%2 else (large[0][0] - small[0][0])/2)
        return ans
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

### Sol: min-max heap, time=log\(n\)

```python
class MedianFinder:
    def __init__(self):
        self.heaps = [], []

    def addNum(self, num):
        small, large = self.heaps
        #push -num to small, then pop small and push to large
        heappush(large, -heappushpop(small, -num))
        #keep balance: remain len(large)=len(small) or len(large)=len(small)+1
        if len(large) > len(small) + 1:
            heappush(small, -heappop(large))

    def findMedian(self):
        small, large = self.heaps
        return large[0] if len(large) > len(small) else (large[0] - small[0]) / 2.0    
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

## 567. Permutation in String

Given two strings **s1** and **s2**, write a function to return true if **s2** contains the permutation of **s1**. In other words, one of the first string's permutations is the **substring** of the second string.

**Example 1:**

```text
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```text
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        c1, c2 = collections.Counter(s1), collections.Counter(s2[:len(s1) - 1])
        for i in range(len(s1) - 1, len(s2)):
            c2[s2[i]] += 1
            if c2 == c1:
                return True
            c2[s2[i - len(s1) + 1]] -= 1
            if c2[s2[i - len(s1) + 1]] == 0:
                del c2[s2[i - len(s1) + 1]]
        return False
```

## 438. Find All Anagrams in a String

Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```text
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```text
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

### Sliding windows

```python
class Solution:
    def findAnagrams(self, s2: str, s1: str) -> List[int]:
        res = []
        c1, c2 = collections.Counter(s1), collections.Counter(s2[:len(s1) - 1])
        for i in range(len(s1) - 1, len(s2)):
            c2[s2[i]] += 1
            if c2 == c1:
                res.append(i - len(s1) + 1) 
            c2[s2[i - len(s1) + 1]] -= 1
            if c2[s2[i - len(s1) + 1]] == 0:
                del c2[s2[i - len(s1) + 1]]
        return res
```

