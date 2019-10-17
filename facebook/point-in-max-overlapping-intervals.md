# Point in max overlapping intervals

Given number **M** and **N** intervals in the form **\[a, b\]** \(inclusive\) where for every interval **-M &lt;= a &lt;= b &lt;= M**, create a program that returns a point where the maximum number of intervals overlap.

Example:

* **M:** 10
* **N:** 4
* Intervals:
  * `[-3, 5]`
  * `[0, 2]`
  * `[8, 10]`
  * `[6, 7]`

A correct answer would be either `0` ,`1` or `2` since those points are found where 2 intervals overlap and 2 is the maximum number of overlapping intervals.

```python
def solve(M, intervals):

    diff = [0] * (2*M+1)

    for s, e in intervals:
        idx = s + M         # -10 -> 0, -9 -> 1, 10 -> 20
        diff[idx] += 1

        e_idx = e + M + 1
        if eidx < 2 * M + 1:
            diff[eidx] -= 1

    prefix = diff
    for i in range(1, 2*M+1):
        prefix[i] += prefix[i - 1]

    max_value = max(prefix)
    max_idx = scores.index(max_value)

    return max_idx - M     # reverse of idx = s + M
```

