# DFS

## 691. Stickers to Spell Word

We are given N different types of stickers. Each sticker has a lowercase English word on it.

You would like to spell out the given `target` string by cutting individual letters from your collection of stickers and rearranging them.

You can use each sticker more than once if you want, and you have infinite quantities of each sticker.

What is the minimum number of stickers that you need to spell out the `target`? If the task is impossible, return -1.

**Example 1:**

Input:

```text
["with", "example", "science"], "thehat"
```

Output:

```text
3
```

Explanation:

```text
We can use 2 "with" stickers, and 1 "example" sticker.
After cutting and rearrange the letters of those stickers, we can form the target "thehat".
Also, this is the minimum number of stickers necessary to form the target string.
```

**Example 2:**

Input:

```text
["notice", "possible"], "basicbasic"
```

Output:

```text
-1
```

Explanation:

```text
We can't form the target "basicbasic" from cutting letters from the given stickers.
```

### Back-tracking

```python
class Solution:
    #back-track
    def minStickers(self, stickers, target):
        # needed:target每个字符出现次数; res是当前最少需要的stickers数量
        needed, res, n = collections.Counter(target), float("inf"), len(target)  
        stickers = [collections.Counter(s) & needed for s in stickers]
        
        # dic是当前选中的所有stickers的字符组成的字典 {字符:出现次数}
        # used是当前使用的stickers的数量
        # i是当前正在判断的target的下标
        def dfs(dic, used, i):
            nonlocal res            
            if i == n: #res保证递减
                res = used              
            # 判断第i位的时候, 如果dic中 target[i]大于等于needed, 判断下一位
            elif dic[target[i]] >= needed[target[i]]: 
                dfs(dic, used, i + 1)                
            # prune: used + 1 < res 
            # 对所有s，如果它包含target[i]；选它，把它加入dic；back-tracking
            elif used + 1 < res:
                for s in stickers:
                    if target[i] in s:
                        dic += s
                        dfs(dic, used + 1, i + 1)
                        dic -= s
                        
        dfs(collections.Counter(), 0, 0)
        return res if res < float("inf") else -1
```

