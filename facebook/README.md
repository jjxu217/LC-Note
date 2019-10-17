# Facebook 面经

**282. Expression Add Operators: back-tracking  
528.** Random Pick with Weight: prefix sum + bisect  
29. Divide Two Integers: 二进制的减法  
269. Alien Dictionary: 拓扑排序  
347. Top K Frequent Elements: heap  
987. Vertical Order Traversal of a Binary Tree: dic record次序  
865. Smallest Subtree with all the Deepest Nodes: Recursion, 返回深度和node  
33. Search in Rotated Sorted Array: bisect  
31. Next Permutation

\*\*\*\*

**609.** Find Duplicate File in System

## **166.** Fraction to Recurring Decima

```python
class Solution:
    def fractionToDecimal(self, numerator, denominator):
        n, remainder = divmod(abs(numerator), abs(denominator))
        sign = '-' if numerator*denominator < 0 else ''
        res = [sign+str(n), '.']
        pos = {}
        while remainder not in pos:
            pos[remainder] = len(res)
            n, remainder = divmod(remainder*10, abs(denominator))
            res.append(str(n))

        res.insert(pos[remainder], '(')
        res.append(')')
        return ''.join(res).replace('(0)', '').rstrip('.')
```

  
358. Rearrange String k Distance Apart

## 767. Reorganize String

**Sort by Count, 出现次数少的放奇数位，多的放偶数位**

```python
class Solution:
    def reorganizeString(self, S):
        N = len(S)
        A = []
        for c, x in sorted((S.count(x), x) for x in set(S)):
            if c > (N+1)//2: return ""
            A.extend(c * x)
        ans = [''] * N
        ans[::2], ans[1::2] = A[N//2:], A[:N//2]
        return "".join(ans)
```

  
1054. Distant Barcodes  
****

{% embed url="https://leetcode.com/discuss/interview-question/354854/Facebook-or-Phone-Screen-or-Cut-Wood" %}

\*\*\*\*[**https://leetcode.com/discuss/interview-question/298483/Facebook-or-Phone-screen-or-Design-HashMap**](https://leetcode.com/discuss/interview-question/298483/Facebook-or-Phone-screen-or-Design-HashMap)\*\*\*\*

\*\*\*\*

