# 1056/1088 Confusing Number

Given a number `N`, return `true` if and only if it is a _confusing number_, which satisfies the following condition:

We can rotate digits by 180 degrees to form new digits. When 0, 1, 6, 8, 9 are rotated 180 degrees, they become 0, 1, 9, 8, 6 respectively. When 2, 3, 4, 5 and 7 are rotated 180 degrees, they become invalid. A _confusing number_ is a number that when rotated 180 degrees becomes a **different** number with each digit valid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/03/23/1268_2.png)

```text
Input: 89
Output: true
Explanation: 
We get 68 after rotating 89, 86 is a valid number and 86!=89.
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/03/26/1268_3.png)

```text
Input: 11
Output: false
Explanation: 
We get 11 after rotating 11, 11 is a valid number but the value remains the same, thus 11 is not a confusing number.
```

**Example 4:**

![](https://assets.leetcode.com/uploads/2019/03/23/1268_4.png)

```text
Input: 25
Output: false
Explanation: 
We get an invalid number after rotating 25.
```

```python
class Solution:
    def confusingNumber(self, N: int) -> bool:
        S = str(N)
        rotation = {"0" : "0", "1" : "1", "6" : "9", "8" : "8", "9" : "6"}
        res = ''
        
        for c in S[::-1]:           # iterate in reverse
            if c not in rotation:
                return False
            res += rotation[c]                
        return res != S
```

## 1088. Confusing Number II

Given a positive integer `N`, return the number of confusing numbers between `1` and `N` inclusive.

**Example 1:**

```text
Input: 20
Output: 6
Explanation: 
The confusing numbers are [6,9,10,16,18,19].
6 converts to 9.
9 converts to 6.
10 converts to 01 which is just 1.
16 converts to 91.
18 converts to 81.
19 converts to 61.
```

**Example 2:**

```text
Input: 100
Output: 19
Explanation: 
The confusing numbers are [6,9,10,16,18,19,60,61,66,68,80,81,86,89,90,91,98,99,100].
```

