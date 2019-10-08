# Chocolate Sweetness

Given an array `chocolate` of `n` non-negative integers, where the values are sweetness levels of the chocolate. You are also given a value `k` which denotes the number of friends you will share this chocolate with. Your friends are greedy so they will always take the highest sweetness chunk. Find out what is the maximum sweetness level you could get.

tltr: Split the array into `k` non-empty continuous subarrays. Write an algorithm to maximize **the minimum** sum among these `k` subarrays.

**Example:**

```text
Input: chocolate = [6, 3, 2, 8, 7, 5], k = 3
Output: 9
Explanation:
The values in array are sweetness level in each chunk of chocolate. Since k = 3, so you have to divide this array in 3 pieces,
such that you would get maximum out of the minimum sweetness level. So, you should divide this array in
[6, 3] -> 6 + 3 = 9
[2, 8] -> 2 + 8 = 10
[7, 5] -> 7 + 5 = 12
Your other two friends will take the sweetest chunk, so they will take 12 and 10. The maximum sweetness level you could get is 9.
```

{% page-ref page="../binary-search/410.-split-array-largest-sum.md" %}

