# Power of 2/3/4

### Solution1:  Iteration,

$$
Time = O(log_b(n))
$$

```python
class Solution:
    def isPowerOfK(self, n: int) -> bool:
        if n <= 0:
        	return False
        while n % k == 0:        	
        	n = n // k
        return n == 1 
```

### Solution2: log

 $$n = k^x$$ ， then $$\frac{log(n)}{log(k)} = x$$ is integer

```python
from math import log
class Solution:
    def isPowerOfK(self, n: int) -> bool:
    	if n <= 0: return False
    	r = log(n)/log(k)
    	return (abs(round(r) - r) < 10**-10)
		
```

### Solution3: **Math derivation**

Find the largest possible number within the range

e.g. the max possible power of two = 2^30 = 1073741824:

```python
return n > 0 && (1073741824 % n == 0);
```

### Solution4: Bit

```python
#power of 2
return n > 0 and (n & (n-1)) == 0

#power of 4
return n>0 and n&(n-1)==0 and len(bin(n)[3:])%2==0

#e.g. 
>>>bin(4**3)
'0b1000000'
```



