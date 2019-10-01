# 1087/1096 Brace Expansion

## 1087. Brace Expansion

A string `S` represents a list of words.

Each letter in the word has 1 or more options.  If there is one option, the letter is represented as is.  If there is more than one option, then curly braces delimit the options.  For example, `"{a,b,c}"` represents options `["a", "b", "c"]`.

For example, `"{a,b,c}d{e,f}"` represents the list `["ade", "adf", "bde", "bdf", "cde", "cdf"]`.

Return all words that can be formed in this manner, in lexicographical order.

**Example 1:**

```text
Input: "{a,b}c{d,e}f"
Output: ["acdf","acef","bcdf","bcef"]
```

**Example 2:**

```text
Input: "abcd"
Output: ["abcd"]
```

```python
class Solution:
    #BFS
    def expand(self, S: str) -> List[str]:
        A = S.replace('{', ' ').replace('}', ' ').strip().split()
        B = [sorted(a.split(',')) for a in A]
        res = ['']
        for batch in B:
            res = [pre + cur for pre in res for cur in batch]
        return res
    
    #DFS, back-tracking
    def expand(self, S):
        res = []
        
        def helper(s, word):
            if not s:
                res.append(word)
            else:
                if s[0] == "{":
                    i = s.find("}")
                    for letter in s[1:i].split(','):
                        helper(s[i+1:], word+letter)
                else:
                    helper(s[1:], word + s[0])
                    
        helper(S, "")
        return sorted(res)
```

## 1096. Brace Expansion II

Under a grammar given below, strings can represent a set of lowercase words.  Let's use `R(expr)` to denote the **set** of words the expression represents.

Grammar can best be understood through simple examples:

* Single letters represent a singleton set containing that word.
  * `R("a") = {"a"}`
  * `R("w") = {"w"}`
* When we take a comma delimited list of 2 or more expressions, we take the union of possibilities.
  * `R("{a,b,c}") = {"a","b","c"}`
  * `R("{{a,b},{b,c}}") = {"a","b","c"}` \(notice the final set only contains each word at most once\)
* When we concatenate two expressions, we take the set of possible concatenations between two words where the first word comes from the first expression and the second word comes from the second expression.
  * `R("{a,b}{c,d}") = {"ac","ad","bc","bd"}`
  * `R("a{b,c}{d,e}f{g,h}") = {"abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"}`

Formally, the 3 rules for our grammar:

* For every lowercase letter `x`, we have `R(x) = {x}`
* For expressions `e_1, e_2, ... , e_k` with `k >= 2`, we have `R({e_1,e_2,...}) = R(e_1) ∪ R(e_2) ∪ ...`
* For expressions `e_1` and `e_2`, we have `R(e_1 + e_2) = {a + b for (a, b) in R(e_1) × R(e_2)}`, where + denotes concatenation, and × denotes the cartesian product.

Given an `expression` representing a set of words under the given grammar, return the sorted list of words that the expression represents.

**Example 1:**

```text
Input: "{a,b}{c,{d,e}}"
Output: ["ac","ad","ae","bc","bd","be"]
```

**Example 2:**

```text
Input: "{{a,z},a{b,c},{ab,z}}"
Output: ["a","ab","ac","z"]
Explanation: Each distinct word is written only once in the final answer.
```

