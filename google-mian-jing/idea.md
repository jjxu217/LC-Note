# idea

## 仓库

给定一堆不同高度的objects和一堆仓库storage的空间。东西只能从左向右推进仓库，如果有个位置太低了，那么后面的位置都会被前面的位置局限住。例如仓库是\[1,5\], 那么第二个位置最多也只能放1。 问最多能把多少objects放进仓库。

例如, objects = \[3,3,1,4,5\] storage = \[4,5,1,5\] 例如这里最多能放3个,\[4,3,0,1\]

**storage的能放的高度是由前面最小的高度决定的，就先遍历了一下storage数组来更新storage能放的高度**。

follow up1 是如果storage的空间远远小于object的数量。 **用heap来弹出最小的object**

follow up2 是如果storage左右都能推进去。 我说是维护一个从左到右的min height和从右往左的min height,然后能放的高度是两个min height中大的那个，他说应该可以

## O-1 matrix，

找出0 1 matrix中是否存在从第一行到最后一行的路径，1才能通过。

follow up1:输出任意路径: DFS

follow up2: 输出最短路径: BFS

follow up3：matrix中的的非零元素代表difficulty， 找出difficulty总和最小的路径。Dijkstra

## Sparse Vector

Suppose we have very large sparse vectors \(most of the elements in vector are zeros\)

1. Find a data structure to store them
2. Compute the Dot Product.

**Follow-up:**  
What if one of the vectors is very small?

```python
a = [(1,2),(2,3),(100,5)]
b = [(0,5),(1,1),(100,6)]

i = 0; j = 0
result = 0
while i < len(a) and j < len(b):
    if a[i][0] == b[j][0]:
        result += a[i][1] * b[j][1]
        i += 1
        j += 1
    elif a[i][0] < b[j][0]:
        i += 1
    else:
        j += 1
print(result)
```

## Non-overlap Interval

给一个数组（只有正整数），给一个数K，找出两段不overlap的interval使得每一段interval里数的和为K，求两段interval长度之和的最小值。 题目有点绕，e.g. A=\[1,2,2,3,1,4\], K=5, 能找到的interval是 \[1,2,2\] & \[1,4\] \[2,3\] & \[1,4\] 返回 4 因为\[2,3\], \[1,4\]是和最小的两段，而不是\[1,2,2\]，\[1,4\]

#### 求前缀和， 先从前往后一遍求前i个数中最短的interval A\[i\]， 再从后往前一遍算后n-i中最短的interval B\[\] 然后对每个i算左右总数最小的

```python
class Solution(object):
        def minSubarray(self, arr, K):
                n=len(arr)
                preSum=0
                A={0:-1}
                B={0:0}

                minlenA=[float('inf')]*n
                localmin=float('inf')
                for i in range(n):
                        preSum+=arr[i]
                        if preSum-K in A:
                                if i-A[preSum-K]<localmin:
                                        minlenA[i]=i-A[preSum-K]
                                        localmin=i-A[preSum-K]
                        A[preSum]=i

                preSum=0
                minlenB=[float('inf')]*n
                localmin=float('inf')
                for j in range(n)[::-1]:
                        preSum+=arr[j]
                        if preSum-K in B:
                                if n-j-B[preSum-K]<localmin:
                                        minlenB[j]=n-j-B[preSum-K]
                                        localmin=n-j-B[preSum-K]
                        B[preSum]=n-j

                res=float('inf')
                for k in range(n):
                        if k==0:
                                cur=minlenB[0]
                        else:
                                cur=minlenA[k-1]+minlenB[k]
                        if cur<res:
                                res=cur

                return -1 if res==float('inf') else res
```

## Diagonal traverse

1. 一个二维矩阵， 按up-right顺序输出

```python
ex：mat=[[1,2,3]
         [4,5,6]
         [7,8,9]], output: 1,4,2,7,5,3,8,6,9
```

followup: what if the vectors in mat is not uniformly sized, namely not a rectangular matrix?

My answer: find the longest vector in mat, and we can do it as a rectangular matrix

```python
class printupright:
    def upright(matrix):
        res = []
        sum1 = 0
        m, n = len(matrix), len(matrix[0])
        while sum1 < m + n -1:
            for i in range(m-1, -1, -1):
                j = sum1 - i
                if 0<= j < n:
                    res.append(matrix[j])
            sum1 += 1
        return res
```

## 二维矩阵最大路径和

给一个二维矩阵，每个元素都是非负整数。找到所有可能路径的最大的和。 在一条路径中，不能包含0，路径可以向上下左右延伸，每个元素只能访问一次。假设矩阵中正整数的路径不可能形成环。

```python
def maxPath(matrix) -> int:
    #dfs with memo
    def dfs(i, j):   
        nonlocal res
        nei = [0, 0, 0, 0]
        seen[i][j] = 1
        for idx, (I, J) in enumerate([(i+1,j),(i-1, j),(i,j+1),(i, j-1)]):
            if 0 <= I < M and 0 <= J < N and matrix[I][J] and seen[I][J] == 0:
                nei[idx] = dfs(I, J)
        nei.sort()
        res = max(res, matrix[i][j] + sum(nei[2:]))
        return matrix[i][j] + nei[-1]

    res = 0
    if not matrix or not matrix[0]: return 0
    M, N = len(matrix), len(matrix[0])
    seen = [[0] * N for _ in range(M)]
    for i in range(M):
        for j in range(N):
            if matrix[i][j] and seen[i][j] == 0:
                dfs(i, j)
    return res
```

## Excel 操作

求get\_reference\_list\("=A10_5_A77\*\(B2+3/C3\)"\) ⇒ \["A10", "A77", "B2", "C3"\] input是一个 Formula string ，返回是一个列表，列表的元素是公式中引用到的Spreadsheet column。 直接从左往右暴力字符串解析就可以搞定了。 

follow up是如果给定spreadsheet的cell和其公式，检查引用关系中是否有circle。 比如 has\_circle\({  
"A1": "=D1+B1+1",  
"B1": "=D1+C1+2",  
"C1": "=D1+ A1+3"  
}\) ⇒ True // circular references between A1, B1, and C

BFS：用每个node 只generate一次，加入set。

## N人排队

N个人站一列，每个人是一个int代表高度。A代表任意一个人，B在A前面。如果AB之间没有比B高的，那么定义A能看到B。让输出列最后那个人能看到的人数。 public int count\(int\[\] people\)

followup1: 定义A能够看到B为，AB之间没有比A高也没有比B高的。输出全部人各能看到几个人。public int\[\] count\(int\[\] people\)   
followup2: 如果列中每个人的高度都不同，怎么处理。   
followup3: 如果有很多人，但是他们身高都差不多，怎么处理。

## Complete binary tree

题目是一个complete binary tree， 所有的node从上到下，从左到右都做了编号，求某个编号是否存在在这个树里

> > 因为左子树的index是parent index的两倍， 右子树的index是两倍加一， 用这个可以逐级推算出每个level的parent的index， 然后验证是否存在

