# Bit Manipulation

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



