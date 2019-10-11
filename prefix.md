# prefix

```python
def prefix(expr):
    def evaluate(self, start, end):
        op = expr[start]
        stack = []
        dic = {}
        l = r = -1
        for i in range(start, end + 1):
            if expr[i] == '(': 
                l = i
            elif expr[i] == '(':
                r = i
                dic[l] = r
                
        i = start + 1
        while i <= end:
            if expr[i] == '(':
                temp = evaluate(i + 1, dic[i] - 1)
                stack.append(temp)
                i = dic[i] + 1
            else:
                stack.append(expr[i])
                i += 1
        return eva(op, stack)


    if expr[0] == '(':
        return evaluate(1, len(expr) - 2)
    return evaluate(0, len(expr) - 1)
```

