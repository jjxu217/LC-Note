# Roman-Integer convert/Integer to English Words

## 12. Roman to Integer

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```text
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

* `I` can be placed before `V` \(5\) and `X` \(10\) to make 4 and 9. 
* `X` can be placed before `L` \(50\) and `C` \(100\) to make 40 and 90. 
* `C` can be placed before `D` \(500\) and `M` \(1000\) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```text
Input: "III"
Output: 3
```

**Example 3:**

```text
Input: "IX"
Output: 9
```

**Example 5:**

```text
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        map = {'I':1, 'V':5, 'X':10, 'L':50, 'C':100, 'D':500, 'M':1000}
        num, pre = 0, 1000
        for cur in [map[j] for j in s]:
            num += cur - 2 * pre if cur > pre else cur
            pre = cur
        return num
```

## 12. Integer to Roman

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```text
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

* `I` can be placed before `V` \(5\) and `X` \(10\) to make 4 and 9. 
* `X` can be placed before `L` \(50\) and `C` \(100\) to make 40 and 90. 
* `C` can be placed before `D` \(500\) and `M` \(1000\) to make 400 and 900.

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```text
Input: 3
Output: "III"
```

**Example 3:**

```text
Input: 9
Output: "IX"
```

**Example 5:**

```text
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        values = [ 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1 ]
        sysbols = [ "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I" ]
        res = ""
        for i, v in enumerate(values):
            res += (num // v) * sysbols[i]
            num %= v
        return res
```

## 273. Integer to English Words

Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 231 - 1.

**Example 1:**

```text
Input: 123
Output: "One Hundred Twenty Three"
```

**Example 2:**

```text
Input: 12345
Output: "Twelve Thousand Three Hundred Forty Five"
```

**Example 3:**

```text
Input: 1234567
Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```

**Example 4:**

```text
Input: 1234567891
Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"
```

### Sol: 从小到大判断，用recursion处理千以上的

In python, arr\[-1:0\] will return empty list; return list, `join` at the end

```python
class Solution:
    def numberToWords(self, num: int) -> str:
        to19 = 'One Two Three Four Five Six Seven Eight Nine Ten Eleven Twelve ' \
           'Thirteen Fourteen Fifteen Sixteen Seventeen Eighteen Nineteen'.split()
        tens = 'Twenty Thirty Forty Fifty Sixty Seventy Eighty Ninety'.split()
        def words(n):
            if num == 0:
                return []
            if num < 20:
                return [to19[num-1]]
            if n < 100:
                return [tens[n//10-2]] + words(n%10)
            if n < 1000:
                return [to19[n//100-1]] + ['Hundred'] + words(n%100)
            for p, w in enumerate(('Thousand', 'Million', 'Billion'), 1):
                if n < 1000 ** (p+1):
                    return words(n // 1000**p) + [w] + words(n % 1000**p)
        return ' '.join(words(num)) or 'Zero'
```

