# Basic

## Global, nonlocal

{% embed url="https://stackoverflow.com/questions/1261875/python-nonlocal-statement" %}

{% embed url="https://www.programiz.com/python-programming/global-local-nonlocal-variables" %}

### 二进制、16进制、8进制 bin\(x\)、hex\(x\)、`oct`\(_x_\)

Convert an integer number to a binary string prefixed with “0b”. The result is a valid Python expression.

```python
>>> bin(3)
'0b11'
>>> bin(-10)
'-0b1010'

>>> hex(255)
'0xff'
>>> hex(-42)
'-0x2a'

>>> oct(8)
'0o10'
>>> oct(-56)
'-0o70'
```

### chr\(i\)

Return the string representing a character whose Unicode code point is the integer _i_. For example, `chr(97)` returns the string `'a'`, while `chr(8364)` returns the string `'€'`. This is the inverse of [`ord()`](https://docs.python.org/3/library/functions.html#ord).

### ord\(c\)

Given a string representing one Unicode character, return an integer representing the Unicode code point of that character. For example, `ord('a')` returns the integer `97` and `ord('€')` \(Euro sign\) returns `8364`. This is the inverse of [`chr()`](https://docs.python.org/3/library/functions.html#chr).

### `filter`\(_function_, _iterable_\)

Construct an iterator from those elements of _iterable_ for which _function_ returns true. _iterable_ may be either a sequence, a container which supports iteration, or an iterator. If _function_ is `None`, the identity function is assumed, that is, all elements of _iterable_ that are false are removed.

Note that `filter(function, iterable)` is equivalent to the generator expression `(item for item in iterable if function(item))` if function is not `None` and `(item for item in iterable if item)` if function is `None`.

