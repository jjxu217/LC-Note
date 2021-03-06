# 对撞类，partition： 75. Sort Colors

## Quick Partition: 

## 75. Sort Colors

Given an array with _n_ objects colored red, white or blue, sort them [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) ****so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note:** You are not suppose to use the library's sort function for this problem.

**Example:**

```text
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Follow up:**

* A rather straight forward solution is a two-pass algorithm using counting sort. First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?

### Sol： 荷兰国旗，3个指针

非常有名的算法，人称“荷兰旗算法” [Dutch national flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem)，经典的 "3-way partitioning"算法，用于解决 quick sort 类排序算法中对于重复元素的健壮性问题，在原有 2-way partitioning 的基础上把所有 val == key 的元素集中于数组中间，实现【（小于），（等于），（大于）】的分区。

* \*\*\*\*[**Princeton 公开课的课件，讲 2-way 和 3-way partitioning**](http://algs4.cs.princeton.edu/lectures/23DemoPartitioning.pdf)\*\*\*\*

![](https://mnmunknown.gitbooks.io/algorithm-notes/partitioning3-overview.png)

![](https://mnmunknown.gitbooks.io/algorithm-notes/3-way.PNG)

* **一张图说明 Dijkstra 的 3-way partitioning，左右指针维护 &lt; key 和 &gt; key 的元素，\[left , cur - 1\] 为 = key 的元素，\[cur, right\] 为未知元素。**
* **只有在和 right 换元素时，cur 指针的位置是不动的，因为下一轮还要看一下换过来的元素是不是 &lt; key 要放到左边。**

**Algorithm**

* Initialize the rightmost boundary of zeros : `p0 = 0`. During the algorithm execution `nums[idx < p0] = 0`.
* Initialize the leftmost boundary of twos : `p2 = n - 1`. During the algorithm execution `nums[idx > p2] = 2`.
* Initialize the index of current element to consider : `curr = 0`.
* While **`curr <= p2`** : **\(equal is also contained\)**
  * If `nums[curr] = 0` : swap `curr`th and `p0`th elements and move both pointers to the right.
  * If `nums[curr] = 2` : swap `curr`th and `p2`th elements. Move pointer `p2` to the left.
  * If `nums[curr] = 1` : move pointer `curr` to the right.

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        p0, cur, p2 = 0, 0, len(nums) - 1
        while cur <= p2:
            if nums[cur] == 0:
                nums[p0], nums[cur] = nums[cur], nums[p0]    
                p0 += 1
                cur += 1
            elif nums[cur] == 2:
                nums[p2], nums[cur] = nums[cur], nums[p2]    
                p2 -= 1
            else:
                cur += 1
```

