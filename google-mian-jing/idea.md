# 扫两遍+数学

## 仓库

给定一堆不同高度的objects和一堆仓库storage的空间。东西只能从左向右推进仓库，如果有个位置太低了，那么后面的位置都会被前面的位置局限住。例如仓库是\[1,5\], 那么第二个位置最多也只能放1。 问最多能把多少objects放进仓库。

例如, objects = \[3,3,1,4,5\] storage = \[4,5,1,5\] 例如这里最多能放3个,\[4,3,0,1\]

**storage的能放的高度是由前面最小的高度决定的，就先遍历了一下storage数组来更新storage能放的高度**。

follow up1 是如果storage的空间远远小于object的数量。 **用heap来弹出最小的object**

follow up2 是如果storage左右都能推进去。 我说是维护一个从左到右的min height和从右往左的min height,然后能放的高度是两个min height中大的那个，他说应该可以

```python
def NumOfObj(obj, storage):
    for i in range(1, len(storage)):
        storage[i] = min(storage[i], storage[i - 1])
    obj.sort()
    i, j = 0, len(storage) - 1
    res = 0
    while i < len(obj) and j >= 0:
        if obj[i] < storage[j]:
            res += 1
            i += 1
            j -= 1
        else:
            j -= 1
    return res
```

## N人排队

N个人站一列，每个人是一个int代表高度。A代表任意一个人，B在A前面。如果AB之间没有比B高的，那么定义A能看到B。让输出列最后那个人能看到的人数。 public int count\(int\[\] people\)

followup1: 定义A能够看到B为，AB之间没有比A高也没有比B高的。输出全部人各能看到几个人。public int\[\] count\(int\[\] people\)   
followup2: 如果列中每个人的高度都不同，怎么处理。   
followup3: 如果有很多人，但是他们身高都差不多，怎么处理。

#### 单调递减栈，前后给扫一遍，算两边的高的

## Sparse Vector

Suppose we have very large sparse vectors \(most of the elements in vector are zeros\)

1. Find a data structure to store them
2. Compute the Dot Product.

**Follow-up:**  
What if one of the vectors is very small?

```python
a = [(1,2),(2,3),(100,5)]
b = [(0,5),(1,1),(100,6)]

i = 0; j = 0
result = 0
while i < len(a) and j < len(b):
    if a[i][0] == b[j][0]:
        result += a[i][1] * b[j][1]
        i += 1
        j += 1
    elif a[i][0] < b[j][0]:
        i += 1
    else:
        j += 1
print(result)
```

## Confusing number

输出1-650中upside down 以后合法的数。 upside down： 6-》9; 61-》 16

如果upside down以后和原来的一样，不输出， 比如11-》11 不输出 1，0，8 upside down以后和原来一样 6-》9 9-》6 有其他字符就是invalid 注意corner case 10-》01 not valid

{% page-ref page="../array/1056-1088-confusing-number.md" %}

## get product

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

## Max square

Given a matrix of size m x n, there exists a square of all 1s in the matrix \(all other entries in the matrix are 0s\). The square of 1s is `sqrt(n)` or greater in size. Find the top left corner of the square and return the size of the square as well.

#### check `(sqrt(n)*i， sqrt(n)*j)` points in the 2D array to see whether there is 1 in it。  Then binary search to find top-left, and bottom-right. Total complexity is O\(\(N/sqrt\(N\)\)^2 + 4\*log\(N\)\) which is simply O\(N\).

## **Delete every alternative node in a circular linked list.**

**Example:**

```text
Input: head -> n1 -> n2 -> n3 -> head
Output: head -> n2 -> head
```

code to delete even positioned nodes

```python
def delete_circular(head):
		head.next = head.next.next
	    temp = head.next
	    while temp!=head and temp.next!=head:
	        temp.next = temp.next.next
```

## char加密

题目是说有一个给字符加密的算法，26个字母都有对应唯一且不同的加密后的字符，比如字母a被加密成y, b被加密成n，等等。 给了两个例子，就是加密前和加密后的句子。我不记得具体的句子是什么了， 但是我有当时记录下的每个字母的对应关系。要求就是写一个程序，输入是一个句子，输出就是按照例子里的加密方式加密后的句子。  
例子里的句子里有4个字母d, n, v, x没有出现过，所以我并不知道他们的具体对应关系，所以也没有办法hard code, 不过剩下没有出现过的字母就是g,h,i,s， 所以肯定是分别对应其中的一个。  
感觉应该是有某种规律，但是我死活想不出来。请问大家有没有什么思路？谢谢！  
‍‌‍‍‍‌‍‌‍‌‌‍‍‍‍‌‍‌‍

| 原 | a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 加密后 | y | n | f | ? | c | w | l | b | k | u | o | m | x | ? | e | v | z | p | d | r | j | ? | t | ? | a | q |

```text
同学，我看有些字符是一个循环的样子，
a -> y.    y -> a
j   -> u,  u -> j
q -> z,  z-> u
所以我猜猜是不是行程一个换就行了？？
i->k->o->e->c->f->w->t->r->p->v->?
g->l->m->x->?                                                       
h->b->n->?               
s->d->?       
所以v对应i， x对应g， n对应h, d对应s。
```

