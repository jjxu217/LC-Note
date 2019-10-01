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

## Fenwick Tree \(Binary Indexed Tree\)

#### 给定 n 个元素 array，时间复杂度为 <a id="&#x7ED9;&#x5B9A;-n-&#x4E2A;&#x5143;&#x7D20;-array&#xFF0C;&#x65F6;&#x95F4;&#x590D;&#x6742;&#x5EA6;&#x4E3A;"></a>

* **build O\(n log n\)**
* **update O\(log n\)**
* **query O\(log n\)**

**Fenwick Tree 主要用于求各种维度的区间 sum，主要缺点在于建树时间长于 segment tree ，需要 O\(n log n\) 时间，还有和面试官解释的时候比较麻烦。。主要优点是好写，而且非常容易扩展到多维情况。**

**图文并茂的视频讲解**

{% embed url="https://www.youtube.com/watch?v=WbafSgetDDk" %}

![](https://mnmunknown.gitbooks.io/algorithm-notes/fenwick_tree_encode.jpg)

![](https://mnmunknown.gitbooks.io/algorithm-notes/fenwick_tree_pic.jpg)

![](../.gitbook/assets/image%20%2814%29.png)

> * **fenwick tree 可以用 array 存，纯靠 bit manipulation 操作。**
> * **tree 有一个 dummy root，所以对于单维度大小为 n 的输入，实际数组会在每一个维度 +1 的 padding.**
> * **因此在每次更新原数组 index 位置的数时，在树上实际的 update 位置是 index + 1.**
> * **树的 update ，更新当前pos val，先计算 diff，再把 diff 传导过去。因此 fenwick tree 有时候需要一个数组去保存所有值，用于计算 diff.**
> * \*\*\*\*
>
>   > ```python
>   > while i < len(self.BIT):
>   >             self.BIT[i] += diff
>   >             i += (i & -i) 
>   > ```

> * **对于给定 index，树的 update 是一个 index 逐渐增加的过程，相对的，树的求和是个不断寻找 parent，index 逐渐减小的过程。**
> * **Query 是相反的过程，算\[0, k\]的和**
> * \*\*\*\*
>
>   ```python
>           res = 0
>           k += 1
>           while k:
>               res += self.BIT[k]
>               k -= (k & -k) # to parent
>   ```

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

```python
class NumArray:

    def __init__(self, nums):
        self.arr = [0] * len(nums)
        self.BIT = [0] * (len(nums) + 1)
        for i, n in enumerate(nums): 
            self.update(i, n)
        
    def update(self, i, val):
        diff, self.arr[i] = val - self.arr[i], val
        i += 1
        while i < len(self.BIT):
            self.BIT[i] += diff
            i += (i & -i) # to next
            
    def sumRange(self, i, j):
        return self.Sum(j) - self.Sum(i - 1)

    def Sum(self, k):
        res = 0
        k += 1
        while k:
            res += self.BIT[k]
            k -= (k & -k) # to parent
        return res
```

## 308. Range Sum Query 2D - Mutable

```python
class NumMatrix(object):
    def __init__(self, matrix):
        if not matrix:
            return
        m, n = len(matrix), len(matrix[0])
        self.matrix, self.bit = [[0]*n for _ in range(m)], [[0]*(n + 1) for _ in range(m + 1)]
        for i in range(m):
            for j in range(n):
                self.update(i, j, matrix[i][j])

    def update(self, row, col, val):
        diff, self.matrix[row][col] = val-self.matrix[row][col], val
        i = row + 1
        while i < len(self.bit):
            j = col + 1
            while j < len(self.bit[0]):
                self.bit[i][j] += diff
                j += (j & -j)
            i += (i & -i)

    def sumRegion(self, row1, col1, row2, col2):
        return self.sumCorner(row2, col2) + self.sumCorner(row1-1, col1-1) - self.sumCorner(row1-1, col2) - self.sumCorner(row2, col1-1)

    def sumCorner(self, row, col):
        res, i = 0, row + 1
        while i:
            j = col + 1
            while j:
                res += self.bit[i][j]
                j -= (j & -j)
            i -= (i & -i)
        return res
```

