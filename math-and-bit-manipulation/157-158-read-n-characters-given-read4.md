# 157/158 Read N Characters Given Read4

## 157. Read N Characters Given Read4

Given a file and assume that you can only read the file using a given method `read4`, implement a method to read _n_ characters.

**Method read4:**

The API `read4` reads 4 consecutive characters from the file, then writes those characters into the buffer array `buf`.

The return value is the number of actual characters read.

Note that `read4()` has its own file pointer, much like `FILE *fp` in C.

**Definition of read4:**

```text
    Parameter:  char[] buf
    Returns:    int

Note: buf[] is destination not source, the results from read4 will be copied to buf[]
```

Below is a high level example of how `read4` works:

```text
File file("abcdefghijk"); // File is "abcdefghijk", initially file pointer (fp) points to 'a'
char[] buf = new char[4]; // Create buffer with enough space to store characters
read4(buf); // read4 returns 4. Now buf = "abcd", fp points to 'e'
read4(buf); // read4 returns 4. Now buf = "efgh", fp points to 'i'
read4(buf); // read4 returns 3. Now buf = "ijk", fp points to end of file
```

```python
class Solution:
    def read(self, buf, n):
        """
        :type buf: Destination buffer (List[str])
        :type n: Number of characters to read (int)
        :rtype: The number of actual characters read (int)
        """      
        idx = 0
        while idx < n:
            buf4 = [""] * 4
            cnt = read4(buf4) # Read file into buf4.
            if not cnt:
                break
            cnt = min(cnt, n - idx)
            buf[idx: idx+cnt] = buf4[:cnt] # Copy from buf4 to buf.
            idx += cnt
        return idx
```

## 158. Read N Characters Given Read4 II - Call multiple times

Difference between this one and last one:

Think that you have 4 chars "a, b, c, d" in the file, and you want to call your function twice like this:  
  1.read\(buf, 1\); // should return 'a'  
  2. read\(buf, 3\); // should return 'b, c, d'  
All the 4 chars will be consumed in the first call. So the tricky part of this question is how can you preserve the remaining 'b, c, d' to the second call.

```python
class Solution:
    def __init__(self):
        self.queue = collections.deque()
        
    def read(self, buf, n):
        idx = 0
        while idx < n:
            buf4 = [""] * 4
            _ = read4(buf4) # Read file into buf4.
            self.queue.extend(buf4)
            cnt = min(len(self.queue), n - idx) #length
            if not cnt:
                break          
            buf[idx: idx+cnt] = [self.queue.popleft() for _ in range(cnt)] # Copy from queue to buf.
            idx += cnt
        return idx
```

