# Sort

You are given a shcedule of tasks to work on. Each tasks has a start and an end time `[start, end]` where `end > start`. Find out for the given schedule:

* in what intervals you are working \(at least 1 task ongoing\)
* in what intervals you are multitasking \(at least 2 tasks ongoing\)

In other words, find union and intersection of a list of intervals. The input is sorted by start time.

**Example:**  
**Input:** \[\[1, 10\], \[2, 6\], \[9, 12\], \[14, 16\], \[16, 17\]\]

**Output union:** \[\[1, 12\], \[14, 17\]\]

{% page-ref page="../sort/sort-intervals.md" %}

**Output intersection:** \[\[2, 6\], \[9, 10\]\]  
**Explanation:**  
![](https://i.imgur.com/nkY1cO9.png)

```python
def interval_intersections(intervals: list) -> list:
    if not intervals:
        return []

    intervals.sort()
    intersections = []
    start, end = intervals[0][0], intervals[0][1]

    for cur_start, cur_end in intervals[1:]:
        if end > cur_start:
            intersections.append([max(start, cur_start), min(end, cur_end)])
            end = max(end, cur_end)
        else:
            start, end = cur_start, cur_end

    return intersections
```

