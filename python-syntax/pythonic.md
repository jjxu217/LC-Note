# Pythonic

## List comprehension 

The following is valid:

```python
min(i for i ___ if __)
min(i if __  else __ for i __ )
```

The following is wrong:

```python
min(i if __ for i ___ )
```

## Maximum 

```python
# 最大值 
float('inf') 
# 最小值 
float('-inf')

#int 值
import sys 
max = sys.maxsize  # = 2 ** 63 - 1 = 9223372036854775807 
min = -sys.maxsize -1
```

```python
x = float('inf')
y = x + 1
## =>y = float('inf')

x = sys.maxsize
y = x + 1
## => y = 2 ** 63 = 9223372036854775808
```

## Reference in list

```python
#modify the element i doesn't change list y 
y = [1,2,3]
for i in y:
    i += 1
#=> y = [1,2,3]
    
s = [[1,2], [3], 4]
for i in s:
    i = []
#=>s = [[1,2], [3], 4]

In contrary:
y = [1,2,3]
for i in range(len(y)):
    y[i] += 1
#=>  y = [2, 3, 4]

s = [[1,2], [3], 4]
for i in range(len(s)):
    s[i] = []
#=>s = [[], [], []]
```

## List Range

```python
#list commprehension, i+k can be larger than len(k)
s = "abcdefgh"
k = 3
s = [s[i:i+3] for i in range(0, len(s), 3)] #=> s = ['abc', 'def', 'gh']

#reversed function can apply to out-of-range
s = ['a', 'b', 'c']
reversed(s[0:10]) #=> s = ['c', 'b', 'a']
s[0:10] = s[0:10:-1] #=> s = []
```

[https://stackoverflow.com/questions/509211/understanding-slice-notation?page=1&tab=votes\#tab-top](https://stackoverflow.com/questions/509211/understanding-slice-notation?page=1&tab=votes#tab-top)

It's pretty simple really:

```text
a[start:stop]  # items start through stop-1
a[start:]      # items start through the rest of the array
a[:stop]       # items from the beginning through stop-1
a[:]           # a copy of the whole array
```

There is also the `step` value, which can be used with any of the above:

```text
a[start:stop:step] # start through not past stop, by step
```

The key point to remember is that the `:stop` value represents the first value that is _not_ in the selected slice. So, the difference between `stop` and `start` is the number of elements selected \(if `step` is 1, the default\).

The other feature is that `start` or `stop` may be a _negative_ number, which means it counts from the end of the array instead of the beginning. So:

```text
a[-1]    # last item in the array
a[-2:]   # last two items in the array
a[:-2]   # everything except the last two items
```

Similarly, `step` may be a negative number:

```text
a[::-1]    # all items in the array, reversed
a[1::-1]   # the first two items, reversed
a[:-3:-1]  # the last two items, reversed
a[-3::-1]  # everything except the last two items, reversed
```

Python is kind to the programmer if there are fewer items than you ask for. For example, if you ask for `a[:-2]` and `a` only contains one element, you get an empty list instead of an error. Sometimes you would prefer the error, so you have to be aware that this may happen.

