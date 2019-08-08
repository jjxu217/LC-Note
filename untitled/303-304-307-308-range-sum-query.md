# 303/304/307/308 Range Sum Query

## 303. Range Sum Query - Immutable

Given an integer array nums, find the sum of the elements between indices i and j \(i ≤ j\), inclusive.

**Example:**  


```text
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**Note:**  


1. You may assume that the array does not change.
2. There are many calls to sumRange function.

```python
class NumArray:
    def __init__(self, nums: List[int]):
        n = len(nums)
        self.prefix = [0] * (n + 1)
        for i in range(n):
            self.prefix[i + 1] = nums[i] + self.prefix[i]

    def sumRange(self, i: int, j: int) -> int:
        return self.prefix[j + 1] - self.prefix[i]
```

## 304. Range Sum Query 2D - Immutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner \(row1, col1\) and lower right corner \(row2, col2\).

![Range Sum Query 2D](https://leetcode.com/static/images/courses/range_sum_query_2d.png)  
The above rectangle \(with the red border\) is defined by \(row1, col1\) = **\(2, 1\)** and \(row2, col2\) = **\(4, 3\)**, which contains sum = **8**.

**Example:**  


```text
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

**Note:**  


1. You may assume that the matrix does not change.
2. There are many calls to sumRegion function.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

```python
class NumMatrix:
    def __init__(self, matrix: List[List[int]]):
        if not matrix or not matrix[0]:
            self.prefix = []
            return None
        m, n = len(matrix), len(matrix[0])
        self.prefix = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(m):
            for j in range(n):
                self.prefix[i + 1][j + 1] = self.prefix[i][j + 1] \
                    + self.prefix[i + 1][j] + matrix[i][j] - self.prefix[i][j]
                
    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        return self.prefix[row2 + 1][col2 + 1]  - self.prefix[row2 + 1][col1] \
            - self.prefix[row1][col2 + 1] + self.prefix[row1][col1] \
                if self.prefix else 0
```

## 307. Range Sum Query - Mutable

Given an integer array nums, find the sum of the elements between indices i and j \(i ≤ j\), inclusive.

The update\(i, val\) function modifies nums by updating the element at index i to val.

**Example:**

```text
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

**Note:**

1. The array is only modifiable by the update function.
2. You may assume the number of calls to update and sumRange function is distributed evenly.

