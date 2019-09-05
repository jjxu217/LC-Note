# Bit Manipulation

## 201. Bitwise AND of Numbers Range

Given a range \[m, n\] where 0 &lt;= m &lt;= n &lt;= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

**Example 1:**

```text
Input: [5,7]
Output: 4
```

**Example 2:**

```text
Input: [0,1]
Output: 0
```

### Idea

First let's think what does bitwise AND do to two numbers, for example \( 0b means base 2\)

```text
4 & 7 = 0b100 & 0b111 = 0b100
5 & 7 = 0b101 & 0b111 = 0b101
5 & 6 = 0b101 & 0b110 = 0b100
```

The operator & is keeping those bits which is set in both number.

For several numbers, the operator & is keeping those bits which is 1 in every number.

In other word, a bit is 0 in any number will result in 0 in the answer's corresponding bit.

Now consider a range

```text
[m = 0bxyz0acd, n=0bxyz1rst]
```

here xyzpacdrst all are digits in base 2.

We can find two numbers that are special in the range \[m, n\]

```text
(1) m' = 0bxyz0111
(2) n' = 0bxyz1000
```

The bitwise AND of all the numbers in range \[m, n\] is just the bitwise AND of the two special number

```text
rangeBitwiseAnd(m, n) = m' & n' = 0bxyz0000
```

This tells us, the bitwise and of the range is keeping the common bits of m and n from left to right until the first bit that they are different, padding zeros for the rest.

```python
#从左往右看，找到最小/最大数 相同bit的右边界
class Solution:
    def rangeBitwiseAnd(self, m: int, n: int) -> int:
        move = 0
        while m != n:
            m >>= 1
            n >>= 1
            move += 1
        return m << move
```

## 136. Single Number

Given a **non-empty** array of integers, every element appears _twice_ except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```text
Input: [2,2,1]
Output: 1
```

**Example 2:**

```text
Input: [4,1,2,1,2]
Output: 4
```

### XOR

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        res = 0
        for ele in nums:
            res ^= ele
        return res
```

## 137. Single Number II

Given a **non-empty** array of integers, every element appears _three_ times except for one, which appears exactly once. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```text
Input: [2,2,3,2]
Output: 3
```

**Example 2:**

```text
Input: [0,1,0,1,0,1,99]
Output: 99
```

{% embed url="http://liadbiz.github.io/leetcode-single-number-problems-summary/" %}

{% embed url="https://blog.csdn.net/wlwh90/article/details/89712795" %}

## 371. Sum of Two Integers



