# Dynamic Programming

### 概念

1. 类似数学归纳法：把大问题的解决方案用比他小的问题（们）来解决。用小问题的solution 构建大问题的solution
2. 与recursion的关系 a. Recursion 从大到小解决问题，不记录任何sub-solution，只要考虑base case + recursive rule b. DP 从小到大解决问题，记录sub-solution: 由size\(&lt;n\)的sub-solution -&gt; size n the solution; base case; induction rule

### 常见问题：

* The longest ascending subarray given a unsorted array, find the length of the longest subarray in which the numbers are in ascending order: -&gt; Base case: M\[0\] = 1 Induction rule: M\[i\] represents from 0-th element to the i-th element, the value of the longest ascending subarray\(**including** at i-th element\) Solution is the maximum in M



### DP 的解题常用方法：

1. 一维的original data\(such as a rope, a word, wood\) 求MAX or MIN\(cut, merge,ect\) 1.1 if the **weight** of each smallest element in the original data is identical/similar a. e.g. **identital**: 1 meter of rope b. e.g. **similar**: a letter, a number Then this kind of problem is usually simple **Linear scan and look back to the previous elements** e.g.:Longest ascending subarray\(when at i, look back at i-1\); Longest ascending subsequence\(when at i, look back at 1,...,i-1\); Cut rope; Cut palindrome 1.2 If the weight are not the same: a. 沙子归并 b. 切木头 **中心开花，\[index = 0,1,2,..N-1\] for each M\[i,j\], we usually need to try out all possible k that\(i &lt; k &lt; j\), M\[i,j\]=max\(M\[i,k\]\) + /-/ \* M\[k,j\] \(for all possible k\)** e.g. Pizza 问题，两头凑
2. 2D的original data 2.1 Matrix问题 2.2 Two string 寻找Minimum Edit Distancem Longest Common Substring/Subsequence, 一般解题方法是S1 的前i个letter和S2的前j个letter比较。Induction rule一般要看M\[i\]\[j\]和M\[i-1\]\[j-1\]之前关系

