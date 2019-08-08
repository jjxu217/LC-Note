# 66/369 Plus One/Plus One Linked List

Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

**Example 1:**

```text
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

**Example 2:**

```text
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        d = 1
        for i in range(len(digits) - 1, -1, -1):
            d, remain = divmod(digits[i] + d, 10)
            digits[i] = remain
            if not d:
                break
        return [1] + digits if d else digits
```

## 369. Plus One Linked List

Given a non-negative integer represented as **non-empty** a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

**Example :**

```text
Input: [1,2,3]
Output: [1,2,4]
```

