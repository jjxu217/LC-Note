# Knapsack

Knapsack problem A set of N items. Each item has a weight, weights\[i\] \(i=1...N\). The knapsack has a weight capacity W. What's the maximum total weight you can get?

## Q1 Naive 0/1 knapsack

M\[i,w\]: boolean, represents whether we can get exactly weight==w, by using th first i items, may not include the i-th item

Base case: M\[0\]\[0\]=True, M\[0,1\]..M\[0,W\]=false   
Induction rule:

![](../.gitbook/assets/image%20%2813%29.png)

![](../.gitbook/assets/image%20%2840%29.png)

![](../.gitbook/assets/image%20%2810%29.png)

Time = O\(NW\)  
Space = O\(NW\) =&gt; O\(W\)

```python
def getMAxTotalWeight(weights:[], W: int):
  res = [[0] * (W + 1) for _ in range(len(weights) + 1)]
  res[0][0] = 1
  global_max = 0
  for i in range(1, len(weights)+1):
    for j in range(W + 1):
    #induction rule
      if weights[i-1] <= j:
        res[i][j] = res[i-1][j] or res[i-1][j-weights[i-1]]
      else:
        res[i][j] = res[i-1][j]
    #update max
      if res[i][j] and j > global_max:
        global_max = j
  return global_max
```

### Q1a Naive 0/1 knapsack, get number of solutions

Total number of unique ways to exactly fill the knapsack

![](../.gitbook/assets/image%20%2830%29.png)

### Q1b Naive 0/1 knapsack, get minimum number of item

![](../.gitbook/assets/image%20%2842%29.png)

## Q2 Classic 0/1 kanpscak problem:

Each item has a weight weights\[i\] \(i=1,..,N\), and a value v\[i\] \(i=1,..,N\). What the **maximum total value** you can get?

![](../.gitbook/assets/image%20%2826%29.png)

Time=O\(NW\)  
Spcae = O\(NW\)=&gt; O\(W\)  
Trice: 对于不要求恰好装满背包的问题， M\[0,1\],...,M\[0,W\]可以初始化为0，结果也是正确的

## Q3 Unbounded knapsack Problem：You can pick the same item multiple times。

Sol1:

![](../.gitbook/assets/image%20%2825%29.png)

![](../.gitbook/assets/image%20%2817%29.png)

Sol2:

![](../.gitbook/assets/image%20%2811%29.png)

![](../.gitbook/assets/image%20%287%29.png)

![](../.gitbook/assets/image%20%2818%29.png)

![](../.gitbook/assets/image%20%2829%29.png)

We can use a single 1-d array

## Q4 Group 0/1 knapsack problem: the items belong to K groups that you can pick at most one item for each group

![](../.gitbook/assets/image%20%2816%29.png)

![](../.gitbook/assets/image%20%2812%29.png)

![](../.gitbook/assets/image%20%2834%29.png)

One special case:

![](../.gitbook/assets/image%20%2841%29.png)

