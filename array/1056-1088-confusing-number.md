# 1056/1088 Confusing Number

## 1056. Confusing Number

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

### Sol：confusing number = total number formed with "01689" - Strobogrammatic Number.

We can use knowledge of combination to calculate the number of confusing number with fix number of bits.  Use **confusing number = total number formed with "01689" - Strobogrammatic Number.**

* **bit2 == 1**: 2 个\('6', '9'\)
* **bits == 2** : count =  4\(1,6,8,9\) \* 5 - 4 \* 1\(rotate of 1st bit\) = 16
* **bits == 3**: count = 4\(1,6,8,9\) \* 5 \* 5 -  4  \* 3\(0,1,8\) \* 1 
* * **bits % 2 == 0** , count = 

  $$
  4 * 5 ^ {bits - 1} - 4 * 5^{bits // 2 - 1}
  $$

* **bits % 2 == 1**, count = 

  $$
  4 * 5 ^ {bits - 1} - 4 * 3 * 5^{bits // 2 - 1}
  $$

And then we use **DFS** to find all numbers **formed with "01689"** and **less than given number** and **with the same number of bits with the given number**.

```python
class Solution:
    # Use combination to calculate number of confusing number with fix number of digits
    def confusingNumberFixBits(self, bits):
        if bits == 1:
            return 2
        if bits == 2:
            return 16
        if bits % 2 == 0:
            return 4*5**(bits-1) - 4*5**(int(bits/2)-1)       
        return 4*5**(bits-1) - 4*3*5**(int(bits/2)-1)
    
    # Check if a string is a confusing number
    def isConfusing(self, string):
        rotated = [self.maps[c] for c in string[::-1]]
        return ''.join(rotated) != string
    
    def confusingNumberII(self, N):
        S = str(N)
        l = len(S)
        count = 0
        for i in range(1, l):
            count += self.confusingNumberFixBits(i)
            
        self.maps = {"0":"0","1":"1","6":"9","8":"8","9":"6"}
    
        # Use DFS to obtain all possible numbers 
        #(1) constructed by "01689"; 
        #(2) less than N; 
        #(3) has the same number of digits with N
        def dfs(string):
            nonlocal l, count, S
            if len(string) == l:
                if string <= S and self.isConfusing(string):
                    count += 1
                return
            
            index = len(string)
            if string > S[:index]:
                return
            
            for c in self.maps:
                if not (index == 0 and c == "0"):
                    dfs(string+c)
        
        dfs("")
        return count
```

