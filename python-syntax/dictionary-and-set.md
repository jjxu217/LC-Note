# dictionary & set

## Counter

### Most common element

```python
c = Counter('abracadabra')
>>> print(c)
Counter({'a': 5, 'b': 5, 'r': 2, 'c': 1, 'd': 1})

>>> c.most_common()
[('a', 5), ('b', 2), ('r', 2), ('c', 1), ('d', 1)]

>>> c.most_common(1)[0][0] if c else None
'a'
```

