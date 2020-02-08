# Graph

## Best First Search

经典算法：Dijkstra's Algorithm, Time = O\(\(v+e\) log v\)

Usages: 点到面的算法\(一个点到图上任一点\), Time N  
Data Structure: priority queue \(Min\_Heap\)

  
**解题思路**：

1. Initial state\(start node\)
2. Node Expansion/Generation rule:
3. Termination condition: 所有点都计算完才停止，也就是p\_queue为空
4. de-duplicate node in 2

### Properties:

1. One node can be expanded once and only once
2. one node can be generated more than once
3. **all the cost of the nodes that are expanded are monotonically non-decreasing**\(所有从priority queue里面pop出来的元素的值是单调非递减 \)
4. time complexity, for a graph with e&gt;&gt;v and e ~ v^2 time is O\(\(v+e\) log v\)
5. when a node is popped out for expansion, **its value is fixed which is equal to the shortest distance from the start node**

## Find the k-th smallest number in the f\(x,y,z\)=3^x \* 5^y \* 7\*z （int x,y,x &gt; 0）

![](../.gitbook/assets/image%20%2829%29.png)

![](../.gitbook/assets/image%20%2829%29.png)

