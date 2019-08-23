# Mulipulation

## 67 Add Binary

Given two binary strings, return their sum \(also a binary string\).

The input strings are both **non-empty** and contains only characters `1` or `0`.

**Example 1:**

```text
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```text
Input: a = "1010", b = "1011"
Output: "10101"
```

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        if len(a)==0: return b
        if len(b)==0: return a
        if a[-1] == '1' and b[-1] == '1':
            return self.addBinary(self.addBinary(a[0:-1],b[0:-1]),'1')+'0'
        if a[-1] == '0' and b[-1] == '0':
            return self.addBinary(a[0:-1],b[0:-1])+'0'
        else:
            return self.addBinary(a[0:-1],b[0:-1])+'1'
        
        
    def addBinary(self, a: str, b: str) -> str:  
        diff = len(a) - len(b)
        if diff > 0:
            b = "0"*diff + b
        if diff < 0:
            a = "0"*-diff + a
        carry = 0
        l = len(a) - 1
        res = ''
        
        while l >= 0:
            s = int(a[l]) + int(b[l]) + carry
            if s == 2:
                carry = 1
                res = '0' + res
            elif s == 3:
                carry = 1
                res = '1' + res
            else:
                res = str(s) + res
                carry = 0
            l -= 1
        if carry:
            res = str(carry) + res
        return res
```

## 43. Multiply Strings

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```text
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```text
Input: num1 = "123", num2 = "456"
Output: "56088"
```

```text

```

