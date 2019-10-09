# idea



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

## O-1 matrix，

找出0 1 matrix中是否存在从第一行到最后一行的路径，1才能通过。

follow up1:输出任意路径: DFS

follow up2: 输出最短路径: BFS

follow up3：matrix中的的非零元素代表difficulty， 找出difficulty总和最小的路径。Dijkstra

## 机器人抢金币

在一个矩形房间里，有障碍和通道用1和0表示。有两个机器人和一个金币。如果机器人足够聪明，速度一样快，问哪个机器人会先拿到金币。我刚开始想从机器人开始BFS，这样要做两次BFS。转念一想从金币开始只用做一次。和面试官沟通后很快写完code。

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

## Non-overlap Interval

给一个数组（只有正整数），给一个数K，找出两段不overlap的interval使得每一段interval里数的和为K，求两段interval长度之和的最小值。 题目有点绕，e.g. A=\[1,2,2,3,1,4\], K=5, 能找到的interval是 \[1,2,2\] & \[1,4\] \[2,3\] & \[1,4\] 返回 4 因为\[2,3\], \[1,4\]是和最小的两段，而不是\[1,2,2\]，\[1,4\]

#### 求前缀和， 先从前往后一遍求前i个数中最短的interval A\[i\]， 再从后往前一遍算后n-i中最短的interval B\[\] 然后对每个i算左右总数最小的

```python
class Solution(object):
        def minSubarray(self, arr, K):
                n=len(arr)
                preSum=0
                A={0:-1}
                B={0:0}

                minlenA=[float('inf')]*n
                localmin=float('inf')
                for i in range(n):
                        preSum+=arr[i]
                        if preSum-K in A:
                                if i-A[preSum-K]<localmin:
                                        minlenA[i]=i-A[preSum-K]
                                        localmin=i-A[preSum-K]
                        A[preSum]=i

                preSum=0
                minlenB=[float('inf')]*n
                localmin=float('inf')
                for j in range(n)[::-1]:
                        preSum+=arr[j]
                        if preSum-K in B:
                                if n-j-B[preSum-K]<localmin:
                                        minlenB[j]=n-j-B[preSum-K]
                                        localmin=n-j-B[preSum-K]
                        B[preSum]=n-j

                res=float('inf')
                for k in range(n):
                        if k==0:
                                cur=minlenB[0]
                        else:
                                cur=minlenA[k-1]+minlenB[k]
                        if cur<res:
                                res=cur

                return -1 if res==float('inf') else res
```

## max path in grid 二维矩阵最大路径和

给一个二维矩阵，每个元素都是非负整数。找到所有可能路径的最大的和。 在一条路径中，不能包含0，路径可以向上下左右延伸，每个元素只能访问一次。假设矩阵中正整数的路径不可能形成环。

```python
def maxPath(matrix) -> int:
    #dfs with memo
    def dfs(i, j):   
        nonlocal res
        nei = [0, 0, 0, 0]
        seen[i][j] = 1
        for idx, (I, J) in enumerate([(i+1,j),(i-1, j),(i,j+1),(i, j-1)]):
            if 0 <= I < M and 0 <= J < N and matrix[I][J] and seen[I][J] == 0:
                nei[idx] = dfs(I, J)
        nei.sort()
        res = max(res, matrix[i][j] + sum(nei[2:]))
        return matrix[i][j] + nei[-1]

    res = 0
    if not matrix or not matrix[0]: return 0
    M, N = len(matrix), len(matrix[0])
    seen = [[0] * N for _ in range(M)]
    for i in range(M):
        for j in range(N):
            if matrix[i][j] and seen[i][j] == 0:
                dfs(i, j)
    return res
```

## Excel 操作

求get\_reference\_list\("=A10_5_A77\*\(B2+3/C3\)"\) ⇒ \["A10", "A77", "B2", "C3"\] input是一个 Formula string ，返回是一个列表，列表的元素是公式中引用到的Spreadsheet column。 直接从左往右暴力字符串解析就可以搞定了。 

follow up是如果给定spreadsheet的cell和其公式，检查引用关系中是否有circle。 比如 has\_circle\({  
"A1": "=D1+B1+1",  
"B1": "=D1+C1+2",  
"C1": "=D1+ A1+3"  
}\) ⇒ True // circular references between A1, B1, and C

BFS：用每个node 只generate一次，加入set。

## N人排队

N个人站一列，每个人是一个int代表高度。A代表任意一个人，B在A前面。如果AB之间没有比B高的，那么定义A能看到B。让输出列最后那个人能看到的人数。 public int count\(int\[\] people\)

followup1: 定义A能够看到B为，AB之间没有比A高也没有比B高的。输出全部人各能看到几个人。public int\[\] count\(int\[\] people\)   
followup2: 如果列中每个人的高度都不同，怎么处理。   
followup3: 如果有很多人，但是他们身高都差不多，怎么处理。

#### 单调递减栈，前后给扫一遍，算两边的高的

## 

## Random Stream

Stream: backiuwcatbeforewerehpqojf Input: \["back", "before", "cat", fore", "were", "for"\] Output: \[0, 10, 7, 12, 16, 12\] Generator: char getNextChar\(\);

给一个random stream，只能用getNextChar\(\);获取下一个char，给一个list of input words，找每个word在stream中出现的位置（出现位置） 假设stream够长，即使是random，也一定会出现每个word

My idea would be to build a trie from the input words. Keep a list of trie nodes as candidates, and for each new character read, try to extend each candidate node. If a word is reached by a node, we immediately know its length.

```python
class TrieNode():
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.pos = -1
        self.length = -1
        
class Trie():
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word, i, length):
        node = self.root
        for w in word:
            node = node.children[w]
        node.pos = i
        node.length = length
        
class StreamIndex:
    def __init__(self, words: List[str]):
        self.t = Trie()
        self.cand = [self.t.root]
        for i, word in enumerate(words):
            self.t.addWord(word, i, len(word))
            
        n = len(word)
        res = [0] * n
        cnt = idx = 0 #已经找到的word数，stream中的idx
        while cnt < n:
            char = getNextChar()      
            nxt_cand = [] #contain the trie node of next candidate
            for c in self.cand:
                if char in c.children:
                    node = c.children[char]
                    nxt_cand.append(node)
                    #如果找到一个词, 相应结果记录其实的index
                    if node.pos >= 0:
                        cnt += 1
                        res[node.pos] = idx - node.length + 1
                        node.pos = -1  
            idx += 1       
            self.cand = nxt_cand + [self.t.root]
        return res
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



## 2个string palindrome

给两个string，相同长度，问能不能在同一个地方切一刀，然后两个string的各半边组成一个palindrome。 比如aabac和xyzaa，可以在aab\|ac和xyz\|aa这里切，然后aab和aa可以组成palindrome。要求是必须切在相同的位置，而且要第一个左边和第二个右边，或者第二个左边和第一个右边组成palindrome。  
followup是给出所有位置，小哥给了提示才用了two pointer做的

```python
def cut2string(s1, s2):
    n = len(s1)
    p1, p2 = 0, n - 1
    while p1 <= p2:
        if s1[p1] == s2[p2]:
            p1 += 1
            p2 -= 1
```

## 131. Palindrome Partitioning

Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.  
Return all possible palindrome partitioning of _s_.

**Example:**

```text
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

### DFS:

```python
    def partition(self, s):
        def dfs(s, path, res):
            if not s:
                res.append(path[:])
                return
            for i in range(1, len(s)+1):
                if s[:i] == s[i-1::-1]:
                    path.append(s[:i])
                    dfs(s[i:], path, res)
                    path.pop()        
        res = []
        dfs(s, [], res)
        return res
```

## Function execution time, multi thread

输入一个list，每个元素包含是一个进程的id和起止时间。 要求输出所有进程的standalone时间（如果有的话）

```text
 input: proc {id1, 100, 200}, {id2, 150, 300}
 return: {id1, 50} {id2, 100}
```

先按其实时间sort proc；  
Iterate all process， 分类  
1. nxt beginning &gt; cur ending: 记录cur, nxt变cur  
2. nxt ending &gt; cur ending: cur时间=nxt beginning - cur beginning,   
3. nxt ending &gt; cur beginning: 

```python
def CPUStandAlone(CPURunTime):
    if len(CPURunTime) < 1: return []
    res = []
    CPURunTime.sort(key=lambda x:x[0])
    cur = [cpuRunTime[0][0], cpuRunTime[0][1], cpuRunTime[0][2], 0]
    
    for beg, end, idx in CPURunTime[1:]:
        if cur[1] <= beg:
            res.append((cur[2], cur[1] - cur[0] + cur[3]))
            cur = [beg, end, idx, 0]
        elif cur[1] <= end:
            res.append((cur[2], beg - cur[0]))
            cur = [cur[1], end, idx, 0]
        elif cur[0] <= end:
            res.append((idx, 0))
            cur = [end, cur[1], cur[2], cur[3] + beg - cur[0]]
    res.append((cur[2], cur[3] + cur[1] - cur[0]))        
    return res    
```

## **继承皇位 Monarchy**

Given an the following interface, implement its methods

```text
interface Monarchy {
  void birth(String child, String parent);
  void death(String name);
  List<String> getOrderOfSuccession();
}
```

```text
             king
          /         \
       a1            a2
      /  \          /  \
    b1   b2       c1    c2
   / \     \
d1    d2    d3
```

Order of Succession: king -&gt; a1 -&gt; b1 -&gt; d1 -&gt; d2 -&gt; b2 -&gt; d3 -&gt; a2 -&gt; c1 -&gt; c2

**Example:**

```text
Input: King(the first monarch) has 3 children Andy, Bob, Catherine. Andy has one child Matthew. Bob has two children Alex and Asha. Catherine has no children. 
Output: [King, Andy, Matthew, Bob, Alex, Asha, Catherine]
```

```python
class Monarch(object):
    def __init__(self):
        self.name = None
        self.childern = []
        self.isAlive = True
        
class Monarchy(object):
    def __init__(self):
        self.firstMonarch = None
        self.monarchs = {}
        
    def birth(self, child, parent):
        # create monarch
        monarch = Monarch()
        monarch.name = child
        # If it's the first monarch
        if(parent == None and self.firstMonarch == None):
            self.firstMonarch = monarch
        else:
            # find parent and add child
            if not (parent in self.monarchs):
                print "Parent not found"
                return
            self.monarchs[parent].childern.append(monarch)
        # Add the monarch to hash table
        self.monarchs[child] = monarch

    def death(self, name):
        self.monarchs[name].isAlive = False

    def preOrder(self, node):
        if(node):
            if(node.isAlive == True):
                # don't print dead people :'(
                print node.name
            for child in node.childern:
                self.preOrder(child)

    def getOrderOfSuccession(self):
        self.preOrder(self.firstMonarch)

# Main
m = Monarchy()
m.birth("king", None)
m.birth("Andy", "king")
m.birth("Bob", "king")
m.birth("Catherine", "king")
m.birth("Matthew" , "Andy")
m.birth("Alex " , "Bob")
m.birth("Asha " , "Bob")
m.getOrderOfSuccession()
```

## 720. Longest Word in Dictionary

Given a list of strings `words` representing an English Dictionary, find the longest word in `words` that can **be built one character at a time by other words** in `words`. If there is more than one possible answer, return the longest word with the smallest lexicographical order.If there is no answer, return the empty string.

**Example 2:**

```text
Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".
```

### sort words; then iterate word, check word\[-1\] in set, add to set. time=O\(n logn\)

```python
class Solution:
    def longestWord(self, words: List[str]) -> str:
        words.sort()
        words_set, res = set(['']), ''
        for word in words:
            if word[:-1] in words_set:
                words_set.add(word)
                if len(word) > len(res):
                    res = word
        return res
```

### 变种：任何地方都能加上char

跟原題一樣的是，必須從字串長度為1的string 開始構建，構建出來的string 也必須在dictionary中，然後找能夠構建出的最長string。 最後輸出是要輸出list of string, 就是整個構建的過程。原題是構建的時候是在string 的最後加上一個character，面試題是加在string的任何一個位置都可以。

\["o","or","ord","word", "world", "p", "ap", "apl", "appe", "apple", "zapple"\] -&gt; 輸出最長的就是 \["p", "ap", "apl", "appe", "apple", "zapple"\] 

**BFS:**  
先把words 按长度从小到大sort。   
从左往右扫words，hashtable记录parent， 初始化 `{‘’: None}`；所有word找小一号的孩子check hashtable，存在的话就把自己放进,key是自己，value是小一号的string。最后hashtable里都是合格string。 用一个global max记录最长长度和word。   
`Time = O(nlogn + n * m) m是单词的最长长度，是len(words)`

```python
def path(words):
    words.sort()
    parent = {'': None}
    max_word = ''
    for w in words:
        for j in range(len(w)):
            if w[:i] + w[i+1:] in parent:
                parent[w] = w[:i] + w[i+1:]
                if len(w) > len(max_word):
                    max_word = w
                break
    res = []
    while max_word:
        res.append(max_word)
        max_word = parent[max_word]
    return res[::-1]
```

## 

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

