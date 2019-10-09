# DP

## 取硬币： DP

有很多堆硬币，每堆里混着各种数值，你只能从上面取，一次取几个都行。现在你一共可以取n个，问最多可以取多少钱。” 

`dp[i][j]`代表从前i个堆里拿j个硬币，最多可拿到的总额。  
算`dp[i][j]`时，在第i堆取k个时，要看在前i-1堆里取j-k个最多能得到多少钱，尝试所有k后，最大‍‌‍‍‍‌‍‌‍‌‌‍‍‍‍‌‍‌‍的那个填到dp\[i\]\[j\]。写code时应该还有很多要check,比如k的取值范围。  
`dp[i][j] = max(dp[i-1][j-k] + sum(money[i][:k]) for k in min(len(money[i], j)`

```python
def getcoin(money, n):
    m = len(money)#一共m堆硬币
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, n + 1):
        for j in range(1, m + 1):

```

## **机器人从左上方到右下方**

目标：从左上角到右下角；每步：右上，右边，右下

`dp[i][j] = dp[i-1][j-1] + dp[i][j-1] + dp[i+1][j-1]`

**Follow up 1: optimize space complexity：滚动数组**

**Follow up 2: give 3 points in the matrix, check whether there is a path that can go through these 3 points**  
sort 3 points by using Y-axis or j if there are 2 points having the same J, return False**；**The range of accessible area len = y\(i\) - y\(i-1\)； upper = x\(i-1\) - len； lower = x\(i-1\) + len

**Follow up 3: give 3 points in the matrix, find out the sum of all paths that can go through 3 points**  
Use DP just as the original problem When reaching a point in the problem, reset every cell to 0

## 818. Race Car

Your car starts at position 0 and speed +1 on an infinite number line.  \(Your car can go into negative positions.\)

Your car drives automatically according to a sequence of instructions A \(accelerate\) and R \(reverse\).

When you get an instruction "A", your car does the following: `position += speed, speed *= 2`.

When you get an instruction "R", your car does the following: if your speed is positive then `speed = -1` , otherwise `speed = 1`.  \(Your position stays the same.\)

For example, after commands "AAR", your car goes to positions 0-&gt;1-&gt;3-&gt;3, and your speed goes to 1-&gt;2-&gt;4-&gt;-1.

Now for some target position, say the **length** of the shortest sequence of instructions to get there.

```text
Example 1:
Input: 
target = 3
Output: 2
Explanation: 
The shortest instruction sequence is "AA".
Your position goes from 0->1->3.
```

```text
Example 2:
Input: 
target = 6
Output: 5
Explanation: 
The shortest instruction sequence is "AAARA".
Your position goes from 0->1->3->7->7->6.
```

### Idea: DP

For the input 5, we can reach with only 7 steps: AARARAA. Because we can step back.  
Let's say `n` is the length of `target` in binary and we have `2 ^ (n - 1) <= target < 2 ^ n`  
We have 2 strategies here  
**1. Go pass our target , stop and turn back**  
We take `n` instructions of `A`.  
`1 + 2 + 4 + ... + 2 ^ (n-1) = 2 ^ n - 1`  
Then we turn back by one `R` instruction.  
In the end, we get closer by `n + 1` instructions.  
**2. Go as far as possible before pass target, stop and turn back**  
We take `N - 1` instruction of `A` and one `R`.  
Then we take `m` instructions of `A`, where `m < n`

#### Consider two general cases for number `i` with bit\_length `n`.

1. `i==2^n-1`, this case, `n` is the best way
2. `2^(n-1)-1<i<2^n-1`, there are two ways to arrive at `i`:
   * Use `n` A to arrive at `2^n-1` first, then use R to change the direction, by here there are `n+1` operations \(n A and one R\), then the remaining is same as `dp[2^n-1-i]`
   * Use `n-1` A to arrive at `2^(n-1)-1` first, then R to change the direction, use `m` A to go backward, then use `R` to change the direction again to move forward, by here there are `n-1+2+m=n+m+1` operations \(`n-1` A, two R, `m` A\) , current position is `2^(n-1)-1-(2^m-1)=2^(n-1)-2^m`, the remaining operations is same as `dp[i-(2^(n-1)-1)+(2^m-1)]=dp[i-2^(n-1)+2^m)]`.

```python
class Solution:
    def __init__(self): 
        self.dp = {0: 0}

    def racecar(self, t):
        if t in self.dp: 
            return self.dp[t]
        n = t.bit_length()
        #case 1: t == 2 ^ n-1
        if 2 ** n - 1 == t: 
            self.dp[t] = n
        #case 2: 2 ^ (n-1) - 1 < i < 2 ^ n - 1
        else:
            #method1: Use n A to arrive at 2 ^ n - 1 first, then use R to change the direction
            self.dp[t] = self.racecar(2**n - 1 - t) + n + 1
            #method2: Use n-1 A to arrive at 2 ^ (n-1)-1 first, then R to change the direction, use m A to go backward, then use R to change the direction again to move forward,
            for m in range(n - 1):
                self.dp[t] = min(self.dp[t], self.racecar(t - 2 ** (n - 1) + 2 ** m) + n + m + 1)
        return self.dp[t]
```

## 837. New 21 Game

Alice plays the following game, loosely based on the card game "21".

Alice starts with `0` points, and draws numbers while she has less than `K` points.  During each draw, she gains an integer number of points randomly from the range `[1, W]`, where `W` is an integer.  Each draw is independent and the outcomes have equal probabilities.

Alice stops drawing numbers when she gets `K` or more points.  What is the probability that she has `N` or less points?

**Example 1:**

```text
Input: N = 10, K = 1, W = 10
Output: 1.00000
Explanation:  Alice gets a single card, then stops.
```

**Example 2:**

```text
Input: N = 6, K = 1, W = 10
Output: 0.60000
Explanation:  Alice gets a single card, then stops.
In 6 out of W = 10 possibilities, she is at or below N = 6 points.
```

**Example 3:**

```text
Input: N = 21, K = 17, W = 10
Output: 0.73278
```

### **Intuition**: DP

The same problems as "Climbing Stairs".

**Explanation**:  
In a word,  
`dp[i]`: probability of get points `i`  
`dp[i] = sum(last W dp values) / W`    
To get `Wsum = sum(last W dp values)`, we can maintain a sliding window with size at most `W`.

**Time Complexity**: O\(N\)

```text
When the game ends, the point is between K and K-1+W
    What is the probability that the the point is less than N?
    - If N is greater than K-1+W, probability is 1
    - If N is less than K, probability is 0
    
    If W == 3 and we want to find the probability to get a 5
    - You can get a card with value 1, 2, or 3 with equal probability (1/3)
    - If you had a 4 and you get a 1: prob(4) * (1/3)
    - If you had a 3 and you get a 2: prob(3) * (1/3)
    - If you had a 2 and you get a 3: prob(2) * (1/3)
    - If you had a 1, you can never reach a 5 in the next draw
    - prob(5) = prob(4) / 3 + prob(3) / 3 + prob(2) /3
    
    To generalize:
    The probability to get point K is
    p(K) = p(K-1) / W + p(K-2) / W + p(K-3) / W + ... p(K-W) / W
    
    let wsum = p(K-1) + p(K-2) + ... + p(K-W)
    p(K) = wsum / W
    
    dp is storing p(i) for i in [0 ... N]
    - We need to maintain the window by
      adding the new p(i), 
      and getting rid of the old p(i-w)
    - check i >= W because we would not have negative points before drawing a card
      For example, we can never get a point of 5 if we drew a card with a value of 6
    - check i < K because we cannot continue the game after reaching K
      For example, if K = 21 and W = 10, the eventual point is between 21 and 30
      To get a point of 27, we can have:
      - a 20 point with a 7
      - a 19 point with a 8
      - a 18 point with a 9
      - a 17 point with a 10
      - but cannot have 21 with a 6 or 22 with a 5 because the game already ends
```

```python
class Solution:
    def new21Game(self, N: int, K: int, W: int) -> float:
        if K == 0 or N >= K + W: return 1
        dp = [1.0] + [0.0] * N
        Wsum = 1.0
        for i in range(1, N + 1):
            dp[i] = Wsum / W
            if i < K: Wsum += dp[i] #we cannot reach i from point larger than K
            if i - W >= 0: Wsum -= dp[i - W] #keep the window size of W
        return sum(dp[K:]) 
```

## 1143. Longest Common Subsequence

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A _subsequence_ of a string is a new string generated from the original string with some characters\(can be none\) deleted without changing the relative order of the remaining characters. \(eg, "ace" is a subsequence of "abcde" while "aec" is not\). A _common subsequence_ of two strings is a subsequence that is common to both strings.

If there is no common subsequence, return 0.

**Example 1:**

```text
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```text
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```text
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

### 2D-DP

```python
class Solution:
    #2d DP
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:     
        dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
        for i, c in enumerate(text1):
            for j, d in enumerate(text2):
                dp[i + 1][j + 1] = 1 + dp[i][j] if c == d else max(dp[i][j + 1], dp[i + 1][j])
        return dp[-1][-1]
    
    #滚动数组
    #use 1 - i % 2 and i % 2 to switch between dp[0] (row 0) and dp[1] (row 1)
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        if m < n:
            return self.longestCommonSubsequence(text2, text1) 
        dp = [[0] * (n + 1) for _ in range(2)]
        for i, c in enumerate(text1):
            for j, d in enumerate(text2):
                dp[1 - i % 2][j + 1] = 1 + dp[i % 2][j] if c == d else max(dp[i % 2][j + 1], dp[1 - i % 2][j])
        return dp[m % 2][-1]
```

## Balanced string 只能有0 和 1组成，不能连续有3个位置上数字相同的string

给定长度n，生成所有给定长度的balanced string

给定长度n，算出一共有多少种balanced string，dp

