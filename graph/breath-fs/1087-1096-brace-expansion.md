# 1087/1096 Brace Expansion

## Itertools.product：cartesian product

```python
def product(*args, repeat=1):
    # product('ABCD', 'xy') --> Ax Ay Bx By Cx Cy Dx Dy
    # product(range(2), repeat=3) --> 000 001 010 011 100 101 110 111
    pools = [tuple(pool) for pool in args] * repeat
    
    result = [[]]
    for pool in pools:
        result = [x+[y] for x in result for y in pool]
        
    for prod in result:
        yield tuple(prod)
```

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

### The general idea

**Split expressions into groups separated by top level `','`; for each top-level sub expression \(substrings with braces\), process it and add it to the corresponding group; finally combine the groups and sort.**

#### Thought process

* in each call of the function, try to remove one level of braces
* maintain a list of groups separated by top level `','`
  * when meets `','`: create a new empty group as the current group
  * when meets `'{'`: check whether current `level` is 0, if so, record the starting index of the sub expression;
    * always increase `level` by 1, no matter whether current level is 0
  * when meets `'}'`: decrease `level` by 1; then check whether current `level` is 0, if so, recursively call `braceExpansionII` to get the set of words from `expresion[start: end]`, where `end` is the current index \(exclusive\).
    * add the expanded word set to the current group
  * when meets a letter: check whether the current `level` is 0, if so, add `[letter]` to the current group
  * **base case: when there is no brace in the expression.**
* finally, after processing all sub expressions and collect all groups \(separated by `','`\), we initialize an empty word\_set, and then iterate through all groups
  * for each group, find the product of the elements inside this group
  * compute the union of all groups
* sort and return
* note that `itertools.product(*group)` returns an iterator of **tuples** of characters, so we use`''.join()` to convert them to strings

```python
class Solution:
    def braceExpansionII(self, expression: str) -> List[str]:
        groups = [[]]
        level = 0
        for i, c in enumerate(expression):
            if c == '{':
                if level == 0:
                    start = i+1
                level += 1
            elif c == '}':
                level -= 1
                if level == 0:
                    groups[-1].append(self.braceExpansionII(expression[start:i]))
            elif c == ',' and level == 0:
                groups.append([])
            elif level == 0:
                groups[-1].append([c])
        word_set = set()
        for group in groups:
            word_set |= set(map(''.join, itertools.product(*group)))
        return sorted(word_set)
```

