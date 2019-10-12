# DP

## 91. Decode Ways

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```text
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

**Example 1:**

```text
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```text
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

### `dp[i]: s[:i]`的编码数量

**Base case**: `dp[0] = 1, dp[i] = 0`  
**Recursion rule**: 

```python
dp[i] += dp[i-1] if s[i - 1] != "0" 
dp[i] += dp[i-2] if i != 1 and "10" <= s[i-2:i] <= "26"
```

```python
class Solution:
        def numDecodings(self, s):      
            dp = [0 for _ in range(len(s)+1)]
            dp[0] = 1
            for i in range(1, len(s)+1):
                if s[i - 1] != "0":
                    dp[i] += dp[i-1]
                if i != 1 and '10' <= s[i-2:i] <= "26":  #"01"ways = 0
                    dp[i] += dp[i-2]
            return dp[-1]
    
    #O(1) space    
        def numDecodings(self, s):
            dp = [0 for i in range(3)]
            dp[0] = 1
            for i in range(1, len(s)+1):        # start from 1
                dp[i % 3] = 0                     #初始化当前位置为0
                if s[i - 1] != "0":
                    dp[i % 3] += dp[(i-1) % 3]
                if i != 1 and '10' <= s[i-2:i] <= "26":
                    dp[i % 3] += dp[(i-2) % 3]
            return dp[len(s) % 3]
```

### Three states DP

* `e0 =` current number of ways we could decode, ending on any number;
* `e1 =` current number of ways we could decode, ending on an open 1;
* `e2 =` current number of ways we could decode, ending on an open 2;

```python
def numDecodings(self, s: str) -> int:
    e0, e1, e2 = 1, 0, 0
    for c in s:
        e0, e1, e2 = (c != '0') * e0 + e1 + (c <= '6') * e2, (c == '1') * e0, (c == '2') * e0
    return e0
```

## 639. Decode Ways II

A message containing letters from `A-Z` is being encoded to numbers using the following mapping way:

```text
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Beyond that, now the encoded string can also contain the character '\*', which can be treated as one of the numbers from 1 to 9. Given the encoded message containing digits and the character '\*', return the total number of ways to decode it. Also, since the answer may be very large, you should return the output mod 10^9 + 7.

**Example 1:**

```text
Input: "*"
Output: 9
Explanation: The encoded message can be decoded to the string: "A", "B", "C", "D", "E", "F", "G", "H", "I".
```

**Example 2:**

```text
Input: "1*"
Output: 9 + 9 = 18
```

### Three states DP: number of decode ending on any number/open 1/open 2

* `e0 =` current number of ways we could decode, ending on any number;
* `e1 =` current number of ways we could decode, ending on an open 1;
* `e2 =` current number of ways we could decode, ending on an open 2;

\(Here, an "open 1" means a 1 that may later be used as the first digit of a 2 digit number, because it has not been used in a previous 2 digit number.\)

Say we see some character `c`. We want to calculate `f0, f1, f2`, the corresponding versions of `e0, e1, e2` after parsing character `c`.

**If `c == '*'`**, then the number of ways to finish in total is:   
**Put \* as a single digit number \(`9*e0`\)** + **pair \* as a 2 digit number `1*` in `9*e1` ways**  **+**  **pair \* as a 2 digit number `2*` in `6*e2` ways**.   
The number of ways to finish with an open 1 \(or 2\) is just `e0`.

**If `c != '*'`**, then the number of ways to finish in total is:   
**Put `c` as a single digit if it is not zero \(`(c!='0')*e0`\)** + **pair `c` with open 1** + **pair `c` with open 2 if it is 6 or less \(`(c<='6')*e2`\).**   
The number of ways to finish with an open 1 \(or 2\) is `e0` iff `c == '1'` \(or `c == '2'`\).

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        MOD = 10**9 + 7
        e0, e1, e2 = 1, 0, 0
        for c in s:
            if c == '*':
                f0 = 9*e0 + 9*e1 + 6*e2
                f1 = e0
                f2 = e0
            else:
                f0 = (c ！= '0') * e0 + e1 + (c <= '6') * e2
                f1 = (c == '1') * e0
                f2 = (c == '2') * e0
            e0, e1, e2 = f0 % MOD, f1, f2
        return e0
```

