# String Manipulation

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
        elif a[-1] == '0' and b[-1] == '0':
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

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        #keep the product array reversed until the very end
        res = [0] * (len(num1) + len(num2))
        for i, e1 in enumerate(reversed(num1)):
            for j, e2 in enumerate(reversed(num2)):
                res[i + j] += int(e1) * int(e2)
                res[i + j + 1] += res[i + j] // 10 #进位
                res[i + j] = res[i + j] % 10
                
        while len(res) > 1 and res[-1] == 0: # remove leading zero
            res.pop()
        return ''.join(map(str, res[::-1]))
```

## 415. Add Strings

Given two non-negative integers `num1` and `num2` represented as string, return the sum of `num1` and `num2`.

```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        res, carry = [], 0
        num1, num2 = list(num1), list(num2)
        while num1 or num2:
            n1 = n2 = 0
            if num1: n1 = int(num1.pop())
            if num2: n2 = int(num2.pop())
            carry, remain = divmod(n1+n2+carry, 10)
            res.append(remain)
        if carry:
            res.append(carry)
        return ''.join(map(str, res[::-1]))
```

