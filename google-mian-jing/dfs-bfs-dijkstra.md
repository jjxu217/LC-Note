# DFS/BFS/Dijkstra

## O-1 matrix，

找出0 1 matrix中是否存在从第一行到最后一行的路径，1才能通过。

follow up1:输出任意路径: DFS

follow up2: 输出最短路径: BFS

follow up3：matrix中的的非零元素代表difficulty， 找出difficulty总和最小的路径。Dijkstra

## 机器人抢金币

在一个矩形房间里，有障碍和通道用1和0表示。有两个机器人和一个金币。如果机器人足够聪明，速度一样快，问哪个机器人会先拿到金币。我刚开始想从机器人开始BFS，这样要做两次BFS。转念一想从金币开始只用做一次。和面试官沟通后很快写完code。

## 

## max path in grid 二维矩阵最大路径和

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

