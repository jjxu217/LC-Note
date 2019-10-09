# 设计Get product/Exam Room

## Get product

設計一個Class 返回 k 個數的乘積 k為定值 這個class 有兩個功能 一個為insert 另一個為 getproduct

假設k = 3 數列為 \[1,2,3\] getproduct 返回6 再insert一個新的數4 最先加入的數 1會被踢除 數列變為 \[2,3,4\] 此時 getproudct 返回 24

followup: 所有操作為O\(1\) queue: 存所有元素； num\_zero: 0的数量； product: 所有非零的数的乘积

如果push进来的数是0， num\_zero++, 否则product \*= queue\[-1\];  
如果pop出去的数是0， num\_zero--, 否则product /= queue.popleft\(\) 或者1 如果不够k个;   
if\(num\_zero &gt;0\) return 0; else return product;

```python
class Solution:
	def __init__(self, K):
		self.zeros = 0
		self.product = 1
		self.queue = collections.deque()
		self.K = K
	
	def insert(self, n):
		self.queue.append(n)
		pre = 1	
		if len(self.queue) > self.K:
			pre = self.queue.popleft()	
					
		if n == 0:
			self.zeros += 1
		else:
			self.product *= n
		
		if pre == 0:
			self.zeros -= 1
		else:
			self.product /= pre		
		
		return 0 if self.zeros else self.product
```

## 855. Exam Room

In an exam room, there are `N` seats in a single row, numbered `0, 1, 2, ..., N-1`.

When a student enters the room, they must sit in the seat that maximizes the distance to the closest person.  If there are multiple such seats, they sit in the seat with the lowest number.  \(Also, if no one is in the room, then the student sits at seat number 0.\)

Return a class `ExamRoom(int N)` that exposes two functions: `ExamRoom.seat()` returning an `int` representing what seat the student sat in, and `ExamRoom.leave(int p)` representing that the student in seat number `p` now leaves the room.  It is guaranteed that any calls to `ExamRoom.leave(p)` have a student sitting in seat `p`.

**Example 1:**

```text
Input: ["ExamRoom","seat","seat","seat","seat","leave","seat"], [[10],[],[],[],[],[4],[]]
Output: [null,0,9,4,2,null,5]
Explanation:
ExamRoom(10) -> null
seat() -> 0, no one is in the room, then the student sits at seat number 0.
seat() -> 9, the student sits at the last seat number 9.
seat() -> 4, the student sits at the last seat number 4.
seat() -> 2, the student sits at the last seat number 2.
leave(4) -> null
seat() -> 5, the student sits at the last seat number 5.
```

### ​​​​​​​Use a list `L` to record the index of seats where people sit.

`seat()`:  
1. find the biggest distance at the start, at the end and in the middle.  
2. insert index of seat  
3. return index

`leave(p)`: pop out `p`

**Time Complexity**: O\(N\) for `seat()` and `leave()`

```python
class ExamRoom:
    def __init__(self, N):
        self.N, self.L = N, []

    def seat(self):
        N, L = self.N, self.L
        if not L: res = 0
        else:
            d, res = L[0], 0 #begin with 0 pos
            for a, b in zip(L, L[1:]):#检查中间位置
                if (b - a) // 2 > d:
                    d, res = (b - a) // 2, (b + a) // 2
            if N - 1 - L[-1] > d: res = N - 1 #检查最后一个位置
        bisect.insort(L, res)
        return res

    def leave(self, p):
        self.L.remove(p)
```

