# 74/240 Search a 2D Matrix

## Search a 2D Matrix

Write an efficient algorithm that searches for a value in an _m_ x _n_ matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

```text
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```

**Example 2:**

```text
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

### Solution

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or not matrix[0]:
            return False
        m, n = len(matrix), len(matrix[0])
        l, r = 0, n * m - 1
        while l <= r:
            mid = l + (r - l) // 2
            if matrix[mid // n][mid % n] == target:
                return True
            elif matrix[mid // n][mid % n] > target:
                r = mid - 1
            else: 
                l = mid + 1
        return FalseSearch a 2D Matrix
```

## Search a 2D Matrix II

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

**Example:**

Consider the following matrix:

```text
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = `5`, return `true`.

Given target = `20`, return `false`.

### Solution1: Search space reduction

```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix or not matrix[0]:
            return False
        m, n = len(matrix) , len(matrix[0]) 
        r, c = 0, n - 1
        while r < m and c > -1:
            if matrix[r][c] == target:
                return True
            elif matrix[r][c] > target:
                c -= 1
            else:
                r += 1
        return False
```

Time = O\(m + n\)

### Solution2: Bisect

We can partition a sorted two-dimensional matrix into four sorted submatrices, two of which might contain `target` and two of which definitely do not.

_Base Case_

If `target` is smaller than the array's smallest element \(found in the top-left corner\) or larger than the array's largest element \(found in the bottom-right corner\), then it definitely is not present.

_Recursive Case_

We seek along the matrix's middle column for an index `row` such that _**matrix\[row-1\]\[mid\] &lt; target &lt; matrix\[row\]\[mid\]**_\(obviously, if we find `target` during this process, we immediately return `true`\). The existing matrix can be partitioned into four submatrice around this index; the top-left and bottom-right submatrice cannot contain `target` \(via the argument outlined in _Base Case_ section\), so we can prune them from the search space. Additionally, the bottom-left and top-right submatrice are sorted two-dimensional matrices, so we can recursively apply this algorithm to them.

```python
class Solution:
    def searchMatrix(self, matrix, target):
        # an empty matrix obviously does not contain `target`
        if not matrix:
            return False

        def search_rec(left, up, right, down):
            # this submatrix has no height or no width.
            if left > right or up > down:
                return False
            # `target` is already larger than the largest element or smaller
            # than the smallest element in this submatrix.
            elif target < matrix[up][left] or target > matrix[down][right]:
                return False

            mid = left + (right-left)//2

            # Locate `row` such that matrix[row-1][mid] < target < matrix[row][mid]
            row = up
            while row <= down and matrix[row][mid] <= target:
                if matrix[row][mid] == target:
                    return True
                row += 1
            
            return search_rec(left, row, mid-1, down) or search_rec(mid+1, up, right, row-1)

        return search_rec(0, 0, len(matrix[0])-1, len(matrix)-1)
```

Time: $$\mathcal{O}(nlgn)$$

If $$x= m*n = n^2 =∣matrix∣$$, then  $$T(x)=2⋅T(x/4)+ \sqrt x​$$

