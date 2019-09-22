# 263/264/313 Ugly Number

## 263. Ugly Number

Write a program to check whether a given number is an ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`.

**Example 1:**

```text
Input: 6
Output: true
Explanation: 6 = 2 × 3
```

**Example 3:**

```text
Input: 14
Output: false 
Explanation: 14 is not ugly since it includes another prime factor 7.
```

```python
class Solution:
    def isUgly(self, num: int) -> bool:
        if num == 0: return False
        for i in (2, 3, 5):
            while num % i == 0:
                num = num // i
        return num == 1       
```

## 264. Ugly Number II

Write a program to find the `n`-th ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`. 

**Example:**

```text
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

```python
import heapq
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        ugly_set = set([1])
        ugly_heap = [1]
        while n > 0:
            cur = heapq.heappop(ugly_heap)
            n -= 1  
            for num in [cur * 2,cur * 3,cur * 5]:
                if num not in ugly_set:
                    ugly_set.add(num)
                    heapq.heappush(ugly_heap,num)               
        return cur
    
    #DP: use three pointers i/j/k, to mark the last ugly number which was multiplied by 2, 3 and 5, correspondingly.
    def nthUglyNumber(self, n: int) -> int:
        res = [1]
        i = j = k = 0
        
        while n > 1:
            val = min(res[i]*2, res[j]*3, res[k]*5 )
            if val == res[i]*2:
                i+=1
            if val == res[j]*3:
                j+=1
            if val == res[k]*5:
                k+=1
            n -= 1
            res.append(val)
        return res[-1]
```

## 313. Super Ugly Number

Write a program to find the `nth` super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list `primes` of size `k`.

**Example:**

```text
Input: n = 12, primes = [2,7,13,19]
Output: 32 
Explanation: [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 
             super ugly numbers given primes = [2,7,13,19] of size 4.
```

```python
class Solution:
    def nthSuperUglyNumber(self, n: int, primes: List[int]) -> int:
        l = len(primes)
        pts = [0] * l
        res = [1]
        while n > 1:
            n -= 1
            val = min(res[pts[i]] * primes[i] for i in range(l))
            for i in range(l):
                if val == res[pts[i]] * primes[i]:
                    pts[i] += 1
            res.append(val)
        return res[-1]
    
    import heapq
    def nthSuperUglyNumber(self, n: int, primes: List[int]) -> int:    
        aux = [1]
        dup = set([1])
        a = 0
        while n > 0:
            cur = heapq.heappop(aux)
            n -= 1
            for p in primes:
                if p * cur not in dup:
                    heapq.heappush(aux, p*cur)
                    dup.add(p*cur)            
        return cur
```

