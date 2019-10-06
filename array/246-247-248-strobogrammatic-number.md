# 246/247/248 Strobogrammatic Number

## 246 Strobogrammatic Number

A strobogrammatic number is a number that looks the same when rotated 180 degrees \(looked at upside down\).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

**Example 1:**

```text
Input:  "69"
Output: true
```

**Example 2:**

```text
Input:  "88"
Output: true
```

**Example 3:**

```text
Input:  "962"
Output: false
```

```python
class Solution:
    def isStrobogrammatic(self, num: str) -> bool:
        dic = {"0":"0", "1":"1", "6":"9", "8":"8", "9":"6"}
        l, r = 0, len(num)-1
        while l <= r:
            if num[l] not in dic or dic[num[l]] != num[r]:
                return False
            l += 1
            r -= 1
        return True
```

## 247. Strobogrammatic Number II

A strobogrammatic number is a number that looks the same when rotated 180 degrees \(looked at upside down\).

Find all strobogrammatic numbers that are of length = n.

**Example:**

```text
Input:  n = 2
Output: ["11","69","88","96"]
```

### Sol: Recursion:

Some observation to the sequence:

n == 1: \[0, 1, 8\]

n == 2: \[11, 88, 69, 96\]

How about n == `3`?  
=&gt; it can be retrieved if you insert `[0, 1, 8]` to the middle of solution of n == `2`

n == `4`?  
=&gt; it can be retrieved if you insert `[11, 88, 69, 96, 00]` to the middle of solution of n == `2`

n == `5`?  
=&gt; it can be retrieved if you insert `[0, 1, 8]` to the middle of solution of n == `4`

the same, for n == `6`, it can be retrieved if you insert `[11, 88, 69, 96, 00]` to the middle of solution of n == `4`

```python
class Solution:
    def findStrobogrammatic(self, n: int) -> List[str]:
        evenMidCandidate = ["11","69","88","96", "00"]
        oddMidCandidate = ["0", "1", "8"]
        if n == 1:
            return oddMidCandidate
        elif n == 2:
            return evenMidCandidate[:-1]
        if n % 2:
            pre, midCandidate = self.findStrobogrammatic(n-1), oddMidCandidate
        else: 
            pre, midCandidate = self.findStrobogrammatic(n-2), evenMidCandidate
        premid = (n - 1) // 2
        return [p[:premid] + c + p[premid:] for c in midCandidate for p in pre]
        
    def findStrobogrammatic(self, n):
        nums = n%2 * list('018') or ['']
        while n > 1:
            n -= 2
            nums = [a + num + b for a, b in '00 11 88 69 96'.split()[n<2:] for num in nums]
        return nums
```

## 248. Strobogrammatic Number III

A strobogrammatic number is a number that looks the same when rotated 180 degrees \(looked at upside down\).

Write a function to count the total strobogrammatic numbers that exist in the range of low &lt;= num &lt;= high.

**Example:**

```text
Input: low = "50", high = "100"
Output: 3 
Explanation: 69, 88, and 96 are three strobogrammatic numbers.
```

```python
class Solution:
    def strobogrammaticInRange(self, low: str, high: str) -> int:
        q, cnt, low, high, ln = ["", "0", "1", "8"], 0, int(low), int(high), len(high)
        while q:
            s = q.pop()
            if s and s[0] != "0" and low <= int(s) <= high: cnt += 1
            q += [l + s + r for l, r in (("8", "8"), ("6", "9"), ("9", "6"), ("1", "1"), ("0", "0")) if len(s) <= ln - 2] 
        return cnt if low != 0 else cnt + 1
```

