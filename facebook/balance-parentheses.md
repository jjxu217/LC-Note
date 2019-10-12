# Balance Parentheses

Given a string `str` consisting of parentheses `(`, `)` and alphanumeric characters. Remove minimum number of paranthesis to make the string valid and return **any** valid result. In a valid string for every opening/closing parentheses there is a matching closing/opening one.

**Example 1:**

```text
Input: "ab(a(c)fg)9)"
Output: "ab(a(c)fg)9" or "ab(a(c)fg9)" or "ab(a(cfg)9)"
```

**Example 2:**

```text
Input: ")a(b)c()("
Output: "a(b)c()"
```

**Example 3:**

```text
Input: ")("
Output: ""
```

**Example 4:**

```text
Input: "a(b))"
Output: "a(b)"
```

**Example 5:**

```text
Input: "(a(c()b)"
Output: "a(c()b)" or "(ac()b)" or "(a(c)b)"
```

**Example 6:**

```text
Input: "(a)b(c)d(e)f)(g)"
Output: "(a)b(c)d(e)f(g)"
```

### 从左往右扫一遍 删除右括号，从右往左扫一遍删除左括号

```python
def balanceParens(string): 
    n = len(string)
    remove = [0] * n
    bal = 0
    for i in range(n):
        if string[i] == '(':
            bal += 1
        elif string[i] == ')':
            if bal > 0:
                bal -= 1
            else: 
                remove[i] = 1
                
    for i in range(n - 1, -1, -1):
        if string[i] == ')':
            bal += 1
        elif string[i] == '(':
            if bal > 0:
                bal -= 1
            else: 
                remove[i] = 1
    return ''.join([string[i] for i in range(n) if not remove[i])
```

{% page-ref page="../graph/breath-fs/301.-remove-invalid-parentheses.md" %}

{% page-ref page="../stack/921-856.-minimum-add-to-make-parentheses-valid.md" %}

