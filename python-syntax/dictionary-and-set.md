# dictionary & set

## 

## del

```python
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]

>>> tel = {'jack': 4098, 'sape': 4139}
>>> del tel['sape']
>>> tel
{'jack': 4098}
```

## dict

### popitem\(\)

Removes the last inserted key-value pair

```python
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'jack': 4098, 'sape': 4139, 'guido': 4127}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['irv'] = 4127
>>> tel
{'jack': 4098, 'guido': 4127, 'irv': 4127}
>>> list(tel)
['jack', 'guido', 'irv']
>>> sorted(tel)
['guido', 'irv', 'jack']
>>> 'guido' in tel
True

>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'guido': 4127, 'jack': 4098}
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
```

## OrderDict\(\)

### `order.popitem(`_`last=True`_`)`

`last=True表示最后一个； last=False表示第一个`

```python
>>> # regular unsorted dictionary
>>> d = {'banana': 3, 'apple': 4, 'pear': 1, 'orange': 2}

>>> # dictionary sorted by key
>>> OrderedDict(sorted(d.items(), key=lambda t: t[0]))
OrderedDict([('apple', 4), ('banana', 3), ('orange', 2), ('pear', 1)])

>>> # dictionary sorted by value
>>> OrderedDict(sorted(d.items(), key=lambda t: t[1]))
OrderedDict([('pear', 1), ('orange', 2), ('banana', 3), ('apple', 4)])

>>> # dictionary sorted by length of the key string
>>> OrderedDict(sorted(d.items(), key=lambda t: len(t[0])))
OrderedDict([('pear', 1), ('apple', 4), ('orange', 2), ('banana', 3)])
```

## set

### discard\(\)

The `discard()` method removes the specified item from the set.  
This method is different from the `remove()` method, because the `remove()` method _will raise an error_ if the specified item does not exist, and the `discard()` method _will not_.

### intersection\(_\*others_\) = set & other

Returns a set, that is the intersection of two other sets

### `set ^ other`

Return a new set with elements in either the set or _other_ but not both.

### `difference`\(_\*others_\) = `set - other - ...`

Return a new set with elements in the set that are not in the others.

### `union`\(_\*others_\)`set | other | ...`

Return a new set with elements from the set and all others.

## Counter

### `most_common`\(\[_n_\]\)

```python
c = Counter('abracadabra')
>>> print(c)
Counter({'a': 5, 'b': 5, 'r': 2, 'c': 1, 'd': 1})

>>> c.most_common(5)
[('a', 5), ('b', 2), ('r', 2), ('c', 1), ('d', 1)]

>>> c.most_common(1)[0][0] if c else None
'a'
```

### element\(\)

```python
>>> c = Counter(a=4, b=2, c=0, d=-2)
>>> sorted(c.elements())
['a', 'a', 'a', 'a', 'b', 'b']
```

### `subtract`\(\[_iterable-or-mapping_\]\)

```python
>>> c = Counter(a=4, b=2, c=0, d=-2)
>>> d = Counter(a=1, b=2, c=3, d=4)
>>> c - d
Counter({'a': 3})
>>> c.subtract(d)
>>> c
Counter({'a': 3, 'b': 0, 'c': -3, 'd': -6})
```

## _class_ `collections.defaultdict`\(\[_default\_factory_\[, _..._\]\]\)

```python
collections.defaultdict(list) = collections.defaultdict(lambda: [])
collections.defaultdict(lambda: ListNode(0))
collections.defaultdict(ListNode)
```

