# 224/227/772 Basic Calculator 150 Evaluate Reverse Polish Notation

## 224. Basic Calculator

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, **non-negative** integers and empty spaces .

**Example 1:**

```text
Input: "1 + 1"
Output: 2
```

**Example 2:**

```text
Input: " 2-1 + 2 "
Output: 3
```

**Example 3:**

```text
Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

### **Algorithm: stack, sign associated with number, time=O\(n\)**

```python
class Solution:
    def calculate(self, s: str) -> int:
        def update(sign, num):
            if sign == '+':
                stack.append(num)
            elif sign == '-':
                stack.append(-num)         
        
        stack = []
        num, sign = 0, '+'
        for i in range(len(s)):
            if s[i].isdigit():
                num = num * 10 + int(s[i])
            elif s[i] in '+-':
                update(sign, num)
                num, sign = 0, s[i]
            elif s[i] == '(':
                stack.append(sign)
                num, sign = 0, '+'
            elif s[i] == ')':
                update(sign, num)
                num = 0
                while isinstance(stack[-1], int):
                    num += stack.pop() 
                sign = stack.pop()
                update(sign, num)
                num, sign = 0, '+'    
            
        update(sign, num)
        return sum(stack)
```

## 227. Basic Calculator II

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only **non-negative** integers, `+`, `-`, `*`, `/` operators and empty spaces . The integer division should truncate toward zero.

**Example 1:**

```text
Input: "3+2*2"
Output: 7
```

**Example 2:**

```text
Input: " 3/2 "
Output: 1
```

**Example 3:**

```text
Input: " 3+5 / 2 "
Output: 5
```

### Sol: use stack to store elements, when encounter sign or the last char, evaluate to the left

```python
class Solution:
    def calculate(self, s: str) -> int:
        def update(sign, num):
            if sign == '+':
                stack.append(num)
            elif sign == '-':
                stack.append(-num)
            elif sign == '*':
                stack.append(stack.pop() * num)
            elif sign == '/':
                stack.append(int(stack.pop() / num))
                
        if not s: return 0
        num, sign, stack = 0, '+', []
        for i in range(len(s)):
            if s[i].isdigit():
                num = 10 * num + int(s[i])
            #when encounter sign evaluate to the left
            if s[i] in '+-*/': 
                update(sign, num)
                sign, num = s[i], 0
        
        #evaluate the last expr
        update(sign, num)
        return sum(stack)
```

## 772. Basic Calculator III

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, **non-negative** integers and empty spaces .

The expression string contains only non-negative integers, `+`, `-`, `*`, `/` operators , open `(` and closing parentheses `)` and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-2147483648, 2147483647]`.

Some examples:

```text
"1 + 1" = 2
" 6-4 / 2 " = 4
"2*(5+5*2)/3+(6/2+8)" = 21
"(2+6* 3+5- (3*14/7+2)*5)+3"=-12
```

```python
class Solution:
    def calculate(self, s: str) -> int:
        def update(sign, num):
            if sign == '+':
                stack.append(num)
            elif sign == '-':
                stack.append(-num)
            elif sign == '*':
                stack.append(stack.pop() * num)
            elif sign == '/':
                stack.append(int(stack.pop() / num))
        
        stack = []
        num, sign = 0, '+'
        for i in range(len(s)):
            if s[i].isdigit():
                num = num * 10 + int(s[i])
            elif s[i] in '+-*/':
                update(sign, num)
                num, sign = 0, s[i]
            elif s[i] == '(':
                #append the sign before '(', reset sign/num to evaluate inside '()'
                stack.append(sign)
                num, sign = 0, '+'
            elif s[i] == ')':
                #evaluate the expr in '()'
                update(sign, num)      
                num = 0
                while isinstance(stack[-1], int):
                    num += stack.pop() 
                sign = stack.pop()
                update(sign, num)
                num, sign = 0, '+'          
            
        update(sign, num)
        return sum(stack)
```

## 150. Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

**Note:**

* Division between two integers should truncate toward zero.
* The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**

```text
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```text
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**

```text
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        def update(sign, num1, num2):
            if sign == '+':
                return num1 + num2
            elif sign == '-':
                return num1 - num2
            elif sign == '*':
                return num1 * num2
            else:
                return int(num1 / num2)
            
        stack = []
        for i in tokens:
            if i in '+-*/':
                temp = int(stack.pop())
                stack.append(update(i, int(stack.pop()), temp))
            else:
                stack.append(i)
        return stack.pop()
```

