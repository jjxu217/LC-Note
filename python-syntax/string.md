# String

## string.join

[https://www.programiz.com/python-programming/methods/string/join](https://www.programiz.com/python-programming/methods/string/join)

```python
numList = ['1', '2', '3', '4']
seperator = ', '
print(seperator.join(numList))
#1, 2, 3, 4 

numTuple = ('1', '2', '3', '4')
print(seperator.join(numTuple))
#1, 2, 3, 4 

s1 = 'abc'
s2 = '123'

""" Each character of s2 is concatenated to the front of s1""" 
print('s1.join(s2):', s1.join(s2))
#s1.join(s2): 1abc2abc3 

""" Each character of s1 is concatenated to the front of s2""" 
print('s2.join(s1):', s2.join(s1))
#s2.join(s1): a123b123c
```

output: 1, 2, 3, 4 

### `str.isalnum`\(\)

Return true if all characters in the string are alphanumeric and there is at least one character, false otherwise.

### `str.isalpha`\(\) ,`str.isdecimal`\(\), `str.islower`\(\), `str.isupper`\(\)

### `str.lower`\(\), `str.upper`\(\)

### `str.replace`\(_old_, _new_\[, _count_\]\)

### `str.split`\(_sep=None_, _maxsplit=-1_\)

```python
>>> '1,2,3'.split(',')
['1', '2', '3']
>>> '1,2,3'.split(',', maxsplit=1)
['1', '2,3']
>>> '1,2,,3,'.split(',')
['1', '2', '', '3', '']

>>> '1 2 3'.split()
['1', '2', '3']
>>> '1 2 3'.split(maxsplit=1)
['1', '2 3']
>>> '   1   2   3   '.split()
['1', '2', '3']
```

### `str.strip`\(\[_chars_\]\)

Return a copy of the string with the leading and trailing characters removed. The _chars_ argument is a string specifying the set of characters to be removed. If omitted or `None`, the _chars_ argument defaults to removing whitespace. The _chars_ argument is not a prefix or suffix; rather, all combinations of its values are stripped:&gt;&gt;&gt;

```python
>>> '   spacious   '.strip()
'spacious'
>>> 'www.example.com'.strip('cmowz.')
'example'
```



