# Google 面经



3 algs

4th: behaviour question, leader, ambiguity

            5th: security domain knowledge, get deep

Steps:

Listen carefully

Ask correct questions

Find multiple solutions before choosing the best one

Seek new ideas and methods

Flexible to new ideas

Can solve even more complex proble

**Dijkstra:**

class Graph:

            def \_\_init\_\_\(self, vertices\):

                        self.V = vertices

                        self.graph = \[\[0 for column in range\(len\(vertices\)\)\] for row in range\(len\(vertices\)\)\]

            def min\_distance\(self, dist, spt\_set\):

                        min\_dis = float\(‘inf’\)

                        for v self.V:

                                    if v not in spt\_set and dist\[v\] &lt; min\_dis:

                                                min\_dis = dist\[v\]

                                                min\_index = v

                        return min\_index

            def dijkstra\(self, src\):

                        dist = \[float\(‘inf’\)\] \* len\(self.V\)

                        dist\[src\] = 0

                        spt\_set = set\(\)

                        for i in range\(len\(self.V\)\):

                                    u = self.min\_distance\(dist, spt\_set\)

                                    spt\_set.add\(u\)

                                    for v in self.V:

                                                if self.graph\[u\]\[v\] &gt; 0 and v not in spt\_set and dist\[v\] &lt; dist\[u\] + graph\[u\]\[v\]:

                                                            dist\[v\] = dist\[u\] + graph\[u\]\[v\]

                        print\(dist\)

def dijkstra\(self, src\):

            visited = set\(\)

            dist = \[float\(‘inf’\)\] \* len\(vertices\)

            dist\[src\] = 0

            for i in range\(len\(vertices\)\):

                        u = self.min\_dist\(dist, visited\)

                        visited.add\(u\)

                        for nei in self.neighbors\[u\]:

                                    if nei not in visited and dist\[nei\] &lt; dist\[u\] + self.neighbors\[u\]\[nei\]:

                                                dist\[nei\] = dist\[u\] + self.neighbors\[u\]\[nei\]

            return dist

**travelling salesman problem \(TSP\)**

a list of cities

the distances between each pair of cities

question: the shortest possible route that visits each city and returns to the original city

**Process/Thread**

Both processes and threads are independent sequence of execution

Process run in separate memory spaces

Threads run in shared memory spaces

**Process resources:**

            registers: instructions, storage address

            program counter: instruction pointer

            stack: stores info about the active subroutines

            heap: dynamically allocated memory

Multiple instances of a single program, each instance is a process

Switching from one process to another requires some time

Thead:

A thread is a unit of execution with a process

A process have have 1 - many threads

![](file:////Users/jjxu/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

locks:

            A lock allows only one thread to enter the part that’s locked

            The lock is not shared with any other processes

mutexes:

A mutex is same as a lock but it can be system wide, can be shared by multiple processes

semaphore:

            Same as mutex, but allows X number of threads to enter

**Deadlock**

a situation where a set of processes are blocked because each process is holding a resource and waiting for another resource acquired by some other process

a deadlock is a state in which each member of a group of actions, is waiting for some other member to release a lock

![](file:////Users/jjxu/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

**Livelock**

occur when two or more processes continually repeat the same interaction in response to changes in the other processes without doing any useful work

These processes are not in the waiting state, and they are running concurrently. This is different from a deadlock because in a deadlock all processes are in the waiting state

A livelock is similar to a deadlock, except that the states of the processes involved in the livelock constantly change with regard to one another, none progressing. Livelock is a special case of resource starvation; the general definition only states that a specific process is not progressing.

**Process Scheduling**

The process scheduling is the activity of the process manager that handles **the removal of the running process from the CPU and the selection of another process** on the basis of a particular strategy

Job queue: all processes in the system

ready queue: a set of all process residing in main memory, ready and waiting to execute

device queue: the processes which are blocked due to unavailability of an I/O device consititute queue

![](file:////Users/jjxu/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

前两轮开始面试官都问了一两个behavior问题，可能是新的形式吧

**LeetCode 560**

class Solution:

    def subarraySum\(self, nums, k\):

        """

        :type nums: List\[int\]

        :type k: int

        :rtype: int

        """

     sums = dict\(\)

     sums\[0\] = 1

     s = 0

     result = 0

     for num in nums:

            s += num

            result += sums.get\(s - k, 0\)

            sums\[s\] = sums.get\(s, 0\) + 1

     return result

**自行车匹配问题**

2D 平面，m个人\(P\)，n辆自行车\(B\), 还有空白\(O\)

1. m &lt; n

2. 不存在两个人到同一辆自行车距离相等

3. 每个人尽量找距离自己最近的车，一旦一辆被占领，其他人只能找别的车

OPOBOOP

OOOOOOO

OOOOOOO

OOOOOOO

BOOBOOB

answer:

讨论什么是最佳匹配

1. 给每个人匹配到车：PQ + Map

2. 全局人车距离最短: 匈牙利算法: [https://blog.csdn.net/u011721440/article/details/38169201](https://blog.csdn.net/u011721440/article/details/38169201)

            二分图最小带权匹配

            KM算法

**二叉树删除边**

LC684，685

**684. Redundant Connection: undirected graph**

union-find

iterate through the edges

if not same root, connect

if the same root already, redundant

**685. Redundant Connection: directed graph**

1. 2 parents, no circle

2. 2 parents, with a circle

3. 1 parent, with a circle

1. Check whether there is a node that has multiple parents.

If yes, store 2 edges: edge 1, edge 2

            union-find on all edges except that 2 edges

            if edge 1 creates a circle, remove it

            else edge 2

If no, union-find and remove the edge that creating a circle

**Remove redundant edge from a binary search tree**

def delete\_edge\(self, root\):

            if root is None:

                        return root

            root = dfs\(root, -float\(‘inf’\), float\(‘inf’\)\)

def dfs\(self, root, left, right\):

            if root is None:

                        return root

            if root.val &lt;= left or root.val &gt;= right:

                        return None

            root.left = self.dfs\(root.left, left, root.val\)

            root.right = self.dfs\(root.right, root.val, right\)

            return root

**机器人从左上方到右下方**

rules:

            目标：从左上角到右下角

            每步：右上，右边，右下

dp\[i\]\[j\] = dp\[i-1\]\[j-1\] + dp\[i\]\[j-1\] + dp\[i+1\]\[j-1\]

**Follow up 1: optimize space complexity**

Only store column j-1

**Follow up 2: give 3 points in the matrix, check whether there is a path that can go through these 3 points**

sort 3 points by using Y-axis or j

            if there are 2 points having the same J, return False

**The range of accessible area**

len = y\(i\) - y\(i-1\)

upper = x\(i-1\) - len

lower = x\(i-1\) + len

**Follow up 3: give 3 points in the matrix, find out the sum of all paths that can go through 3 points**

Use DP just as the original problem

When reaching a point in the problem, reset every cell to 0

**Guess Word**

More like a strategy instead of the correct answer

Take a word from the wordlist and guess it

Get the matches of this word

Update our wordlist and keep only the same matches to our guess

For example, we guess “aaaaaa”

if we get matches = 3

we keep the words with exactly 3 a

def match\(self, a, b\):

            matches = 0

            for i in range\(6 or len\(a\)\):

                        if a\[i\] == b\[i\]:

                                    matches += 1

            return matches

def find\_secret\(self, wordlist, master\):

            n = 0

            while n &lt; 6:

                        guess = random.choice\(wordlist\)

                        n = master.guess\(guess\)

                        wordlist = \[w for w in wordlist if self.match\(w, guess\) == n\]

We start with a word with minimal words of 0 matches

**LC890 Word Pattern Match**

input: a wordlist & pattern

return: list 中match pattern的word

def find\_and\_replace\_pattern\(self, words, pattern\):

def match\(word, pattern\):

                        m1, m2 = {}, {}

                        for w, p in zip\(word, pattern\):

                        if w not in m1: m1\[w\] = p

                        if p not in m2: m2\[p\] = w

                        if m1\[w\] != p or m2\[p\] != w:

                                    return False

            return True

return sum\(match\(word, pattern\) for word in words\)

**LC489 Robot Cleaner**

class Solution:

            def clean\_room\(self, robot\):

                        def backtrack\(cell=\(0,0\), d=0\):

                                    visited.add\(cell\)

                                    robot.clean\(\)

                                    for i in range\(4\):

                                                new\_d = \(d + i\) % 4

                                                new\_cell = \(

cell\[0\] + directions\[new\_d\]\[0\],

cell\[1\] + directions\[new\_d\]\[1\]

\)

                                                if new cell not in visited and robot.move\(\):

                                                            backtrack\(new\_cell, new\_d\): 

robot.turn\_right\(\)

                                    robot.turn\_right\(\)

                                    robot.move\(\)

                                    robot.turn\_right\(\)

                                    robot.turn\_right\(\)

                                                robot.turn\_right\(\)

                        directions = \[\(-1, 0\), \(0, 1\), \(1, 0\), \(0, -1\)\]

                        visited = set\(\)

                        backtrack\(\(0, 0\), 0\)

**LC855 Exam Room:** **尽量分开坐，学生可能离开**

class ExamRoom:

            def \_\_init\_\_\(self, N\):

                        self.N = N

                        self.students = \[\]

            def seat\(self\):

                        if len\(self.students\) == 0:

                                    student = 0

                        else:

                                    dist, student = self.students\[0\], 0

                                    for i, s in enumerate\(self.students\):

                                                if i:

                                                            prev = self.students\[i-1\]

                                                            d = \(s - prev\) / 2

                                                            if d &gt; dist:

                                                                        dist, student = d, prev + d

                                    d = self.N - 1 - self.students\[-1\]

                                    if d &gt; dist:

                                                student = self.N - 1

                        bisect.insort\(self.students, student\)

                        return student

            def leave\(self, p\):

                        self.students.remove\(p\)

**Key有过期时间的HashMap**

HashMap with expiring entries

2 hash map:

一个记录key value pair

一个记录key 的过期时间

**不重复长方形随机取点**

一个list的长方形，每个长方形面积不一样，取点，取点概率一样，不因为长方形大小而不同

LC850/ LC497/ LC528

**LC850 Rectangle Area II**

class Solution:

            def ractangle\_area\(self, rectangles\):

                        def intersect\(rec1, rec2\):

                                    return \(

max\(rec1\[0\], rec2\[0\]\),

max\(rec1\[1\], rec2\[1\]\),

min\(rec1\[2\], rec2\[2\]\),

min\(rec1\[3\], rec2\[3\]\)

\)

                        def area\(rec\):

                                    dx = max\(0, rec\[2\] - rec\[0\]\)

                                    dy = max\(0, rec\[3\] - rec\[1\]\)

                                    return dx \* dy

                        ans = 0

                        for rec in rectangles:

                                    ans += area\(rec\)

                        for i in range\(len\(rectangles\)\):

                                    rec1 = rectangles\[i\]

                                    for j in range\(i+1, len\(rectangles\)\):

                                                rec2 = rectangles\[j\]

                                                ans -= area\(intersect\(rec1, rec2\)\)

                        return ans

LC497 Random Point in Non-overlapping rectangles

Prefix + Binary Search

Use point number in each rectangle as weight

Prefix + Binary Search to choose rectangle

Use random to get a point in that rectangle

**LC853 Car Fleet**

def car\_fleet\(target, position, speed\):

            cars = sorted\(zip\(position, speed\)\)

            times = \[\(target - p\)/s for p, s in cars\]

            ans = 0

            while len\(times\) &gt; 1:

                        lead = times.pop\(\)

                        if lead &lt; times\[-1\]:

                                    ans += 1

else:

            times\[-1\] = lead

            return ans + bool\(times\)

**LC857  雇人工**

import heapq

def min\_cost\_to\_hire\_workers\(self, quality, wage, K\):

            workers = sorted\(

            \(w/q, q, w\) for q, w in zip\(quality, wage\)

\)

ans = float\(‘inf’\)

pool = \[\]

sumq = 0

for raito, q, w in workers:

            heapq.heappush\(pool, -q\)

            sumq += q

            if len\(pool\) &gt; K:

                        sumq += heapq.heappop\(pool\)

            if len\(pool\) == K:

                        ans = min\(ans, ratio \* sumq\)

return ans

**LeetCode 750 Corner Rectangle**

class Solution:

            def count\_corner\_rectangles\(self, grid\):

                        count = collections.Counter\(\)

                        ans = 0

                        for row in grid:

                                    for c1, v1 in enumerate\(row\):

                                                if v1:

                                                            for c2 in range\(c1 + 1, len\(row\)\):

                                                                        if row\[c2\]:

                                                                                    ans += count\[c1, c2\]

                                                                                    count\[c1, c2\] += 1

                        return ans

**LeetCode 815 Bus Route 30**

import collections

def num\_buses\_to\_destination\(self, routes, S, T\):

            if S == T:

                        return 0

            routes = \[set\(route\) for route in routes\]

            graph = collections.defaultdict\(set\)

            for i, r1 in enumerate\(routes\):

                        for j in range\(i+1, len\(routes\)\):

                                    r2 = routes\[j\]

                                    if len\(r1 & r2\) &gt; 0:

                                                graph\[r1\].add\(r2\)

                                                graph\[r2\].add\(r1\)

            seen = set\(\)

            target = set\(\)

            for bus, route in enumerate\(routes\):

                        if S in route:

                                    seen.add\(bus\)

                        if T in route:

                                    target.add\(bus\)

            queue = \[\(bus, 1\) for node in seen\]

            for bus, depth in queue:

                        if bus in target:

                                    return depth

                        for nei in graph\[bus\]:

                                    if nei not in seen:

                                                seen.add\(nei\)

                                                queue.append\(\(nei, depth + 1\)\)

            return -1

**14 LeetCode 659 Split array into consecutive subsequences**

import collections

class Solution:

            def isPossible\(self, nums\):

                        count = collections.Counter\(nums\)

                        tails = collections.Counter\(\)

                        for x in nums:

                                    if count\[x\] == 0:

                                                continue

                                    elif tails\[x\] &gt; 0:

                                                tails\[x\] -= 1

                                                tails\[x+1\] += 1

                                    elif count\[x+1\] &gt; 0 and count\[x+2\] &gt; 0:

                                                count\[x+1\] -= 1

                                                count\[x+2\] -= 1

                                                tails\[x+3\] += 1

                                    else:

                                                return False

                                    count\[x\] -= 1

                        return True

**15** **王位继承**

class Node:

            def \_\_init\_\_\(self, name=’’, children=\[\]\):

                        self.name = name

                        self.children = children

self.isdead = False

class KingHeri:

            def \_\_init\_\_\(self, rootname=’king’\):

                        self.root = Node\(rootname\)

                        self.map = {rootname: self.root}

            def birth\(self, parent, child\):

                        self.map\[parent\].children.append\(Node\(child\)\)

            def death\(self, name\):

                        self.map\[name\].isdead = True

            def get\_order\(self\):    

                        res = \[\]

                        self.dfs\(self.root, res\)

                        return res

            def dfs\(self, root, res\):

                        if root is not None and not root.isdead:

                                    res.append\(root.name\)

                        for child in root.children:

                                    self.dfs\(child, res\)

**16 LC951** **树的同构问题**

def flip\_equiv\(root1, root2\):

            if root1 is None:

                        return root2 is None

            if root 2 is None:

                        return root1 is None

            if root1.val != root2.val:

                        return False

            return \(

\(flip\_equiv\(root1.left, root2.left\) and flip\_equiv\(root1.right, root2.right\)\)

or

\(flip\_equiv\(root1.left, root2.right\) and flip\_equiv\(root1.right, root2.left\)\)

\)

**17 N叉树**

def del\_nodes\(self, root, to\_delete\):

            to\_delete = set\(to\_delete\)

            res = \[\]

            def helper\(root, is\_root\):

                        if root is None:

                                    return root

                        is\_deleted = root.val in to\_delete

                        if is\_root and not is\_deleted:

                                    res.append\(root\)

                        root.left = helper\(root.left, is\_deleted\)

                        root.right = helper\(root.right, is\_deleted\)

                        if is\_deleted:

                                    return None

                        return root

helper\(root, True\)

return res       

**18** **可乐饮料机**

def coke\(buttons, target\):

           **** m = target\[0\]

            n = target\[1\]

            dp = \[\[False\] \* \(n + 1\) for \_ in range\(m+1\)\]

            for i in range\(m+1\):

                        for j in range\(n+1\):

                                    for button in buttons:

                                                preL = i - button\[0\]

                                                preR = j - button\[1\]

                                                if preL &lt;= 0 and preR &gt;= 0:

                                                            dp\[i\]\[j\] = True

                                                            break

                                                elif preL &gt;= 0 and preR &gt;=0 and dp\[preL\]\[preR\]:

                                                            dp\[i\]\[j\] = True

                                                            break

            return dp\[-1\]\[-1\]

**写程序判断一个数是不是自我描述数，以及找出所有这种类型的数字**

import collections

def is\_self\_decribing\(n\):

            s = str\(n\)

            count = colletions.Counter\(\[ch for ch in s\]\)

            for i, ch in enumerate\(s\):

                        if count\[str\(i\)\] != int\(ch\):

                                    return False

            return True

枚举所有这种类型的数字

**LeetCode 947: Most Stones Removed with Same Row or Column**

len\(stones\) - number\_of\_islands

**LeetCode 210 Course Schedule II**

Use DFS to check if a circle exists

If there is a circle, replace every item in the circle with one item

**LeetCode 205 Isomorphic Strings**

**题目：给一个matrix，每个cell是0表示是墙，1表示可以通过，问能不能找到一条路径从第一行的任意格子到最后一行的任意格子，只能走上下左右的路径，不能斜对角；我一开始脑抽用dp写了一遍，小哥给出反例，我就发现还是只能dfs做，然后又写了一遍dfs；follow up是如果能找到路径，输出路径怎么走，这个只要在前一个代码上改动几行就行**

DFS

Balanced string 只能有0 和 1组成，不能连续有3个位置上数字相同的string

给定长度n，生成所有给定长度的balanced string

给定长度n，算出一共有多少种balanced string，dp

**题目：给一个complete binary tree，每个node标记一个index 1，** **2，** **3，4等等，比如1是根节点，2是1的左孩子，3是1的右孩子。第一问给定root node和index n，要求找出这个树中是否存在index为n的node。我一开始说遍历整个树，面试官说可以借助index优化，我就想到先通过把n不停除以2直到1，算出应该的遍历path是经过了哪些index的node，然后根据算出的path直接找就行了，时间复杂度O\(log n\)。**

**第二问是算出这个树中一共有多少node，可以借用前一个写的function也可以不用。这一问实际上是LC 222，但我是按借用前一个function做的，只要先算出树高，然后对最后一层可能出现的所有index，二分查找每个节点是否存在，最后判断停下的节点有没有出现过，输出答案就行，时间复杂度是O\(log^2\)。**

**题目：**

**给一个长度为n的array，元素都是任意的正整数，假设有三个player A，B，C，按顺序轮流取一个格子。A总是从index 0开始取，B总是从index n - 1开始取，C可以从其他任意一个格子开始取，有人取过的格子就不能再取，每次取的格子必须是和这个player取过的格子相邻的某个格子，如果有player取不了格子了他就不取，剩下的人继续取。最后每个人取的所有格子里的数字加起来，就是每个人的得分，得分最高的人获胜。问题是判断C有没有可能获胜。**

**给一个例子，arr = \[5, 5, 5, 6, 6, 6, 7, 2, 3\]，n = 9，我们可以让C从arr\[4\]开始取，然后大家按照如下的策略来取：**

**第一轮，A取arr\[0\] = 5，B取arr\[8\] = 3，C取arr\[4\] = 6**

**第二轮，A取arr\[1\] = 5，B取arr\[7\] = 2，C取arr\[3\] = 6**

**第三轮，A取arr\[2\] = 5，B取arr\[6\] = 7，C取arr\[5\] = 6**

**因为没有人可以再取格子，我们计算总分A = 15，B = 12，C = 18，所以C可以获胜**

希望有人能找到原题在哪里，解法挺复杂的，我也没想好怎么证明，代码是写了二三十行就够了。大概的做法是我们不用管C怎么取，只要列出所有可能的final state，计算其中是否有一种C可以赢。比如说A取的最少的时候，一定是C从arr\[1\]开始，而A取的最多的时候，一定是C从arr\[n-2\]开始，然后列举A取几格的时候，B和C各自可以取多少格，这里还要考虑到根据依次取的规则，C一定和其中一个player取的格子数量相差不超过1，而另一个player可能取很少之类的。比如有10个格子的时候，如果A取到5格，那么C一定要取4格，B取1格，而C取3格，B取2格的情况是不存在的，并不能找到一种存在的取法。

后来跟别人聊了这题以后，又听到了几种思路：

1. 可以用state\(c\_left, c\_right\)来搜索，表示C最左和最右目前为止取到的index，然后列举到目前为止A和B各取了几格，从中选一种C获利最低的取法存入state\(c\_left, c\_right\)，然后在最后格子都被取完了的state结果中选一个C获利最大的，如果这时候C能获胜的话就说明C有获胜的办法。总体上就是min-max游戏的模板，但是这样解效率可能比较低

2. 大部分情况下只要考虑C取的格子数量和A或B一样就可以了，因为如果C和A或B的格子数相差为1的话，那么C只要改变取的起始位置，就可以使取得的格子数量一样，且C的得分更‍‍‍‍‍‌‍‍‌‌‍‍‍‍‍‌‍高，除了arr\[1\]和arr\[n-2\]的时候要特殊考虑。比如n = 10的时候，应该只有这4种情况：ACCCCBBBBB，AACCCCBBBB，AAAACCCCBB和AAAAACCCCB。对A能取到的所有位置，我们用\(n-1\)/2可以计算出C能得到的最大格子数量

**BST validation**

**变量名字取得好一些**

**LeetCode 959 Regions Cut By Slashes**

Split each square into 4 triangles

Connect 4 nodes if the grid is “ ”

Connect two pairs if the grid is “/” or “\”

Connect neighboring nodes

class DSU:

            def \_\_init\_\_\(self, N\):

                        self.p = range\(N\)

            def find\(self, x\):

                        if self.p\[x\] != x:

                                    self.p\[x\] = self.find\(self.p\[x\]\)

                        return self.p\[x\]

            def union\(self, x, y\):

                        xr = self.find\(x\)

                        yr = self.find\(y\)

                        self.p\[xr\] = yr

class Solution:

            def region\_by\_slashes\(self, grid\):

                        N = len\(grid\)

                        dsu = DSU\(4 \* N \* N\)

                        for r, row in enumerate\(grid\):

                                    for c, val in enumerate\(row\):

                                                root = 4 \* \(r \*N + c\)

                                                if val == ‘ ‘:

                                                            dsu.union\(root + 0, root + 1\)

                                                            dsu.union\(root + 2, root + 3\)

                                                            dsu.union\(root + 0, root + 2\)

                                                            dus.union\(root + 1, root + 3\)

**输入一个list，每个元素包含是一个进程的id和起止时间。**

**要求输出所有进程的standalone时间（如果有的话）**

**Bucket Sort**

Bucket sort is useful when the input is uniformly distributed over a range

**Matrix look for biggest rectangle: conner rectangle**

**Sector API**

给一个分区ID的range，内部所有分区都设置成unavailable

给一个分区的ID，求这个ID之后第一个可用的分区

**设计一个class返回k个数字的乘积，insert和get\_product**

class Solution:

            def \_\_init\_\_\(self, K\):

                        self.zeros = 0

                        self.product = 1

                        self.queue = \[\]

                        self.K = K

            def insert\(self, n\):

self.queue.append\(n\)

                        if n == 0:

                                    self.zeros += 1

                                    if len\(self.queue\) &gt; self.K:

                                                self.queue = self.queue\[1:\]

                        else:

                                    self.product \*= n

                                    if len\(self.queue\) &gt; self.K:

pre = self.queue\[0\]     

self.queue = self.queue\[1:\]

                                                self.product /= pre

**LeetCode 739 Daily Temperatures**

Increasing stack

**LeetCode 743 Network Delay Time**

There are N network nodes labelled 1 to N

Given times, a list of travel times as directed edges times\[i\] = \(u, v, w\), where u is the source node, v is the target node, and w is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node K. How long will it take for all nodes to receive the signal? If it is impossible, return -1.

Dijkstra

**Number of Meeting Rooms**

**LeetCode 911 Online Election**

**Lowest Common Ancestor**

递归法

递归法比较直观简单，思路如下：

首先判定当前节点root是否是A节点或者B节点，若是的话直接返回该节点

若不是，分别对root节点的左右子树进行递归查找最小公共父节点，若左右子树都返回了节点，那么表示当前节点就是最小公共父节点，若只有其中一个子树返回了结果，那么就返回该结果节点。

参考代码如下：

TreeNode\* lowestCommonAncestor\(TreeNode\* root, TreeNode\* p, TreeNode\* q\) {

        if\(!root\)

            return NULL;

        if\(root == p \|\| root == q\)

            return root;

        TreeNode\* left = lowestCommonAncestor\(root-&gt;left,p,q\);

        TreeNode\* right = lowestCommonAncestor\(root-&gt;right,p,q\);

        if\(left == NULL\) return right;

        if\(right==NULL\) return left;

        return root;

    }

**Online store price coming in as stream, implement the insert & query by percentile methods**

Binary Search Tree, Each node has val and n \(n is how many nodes in this tree\)

**无向连接图** **return the com‍‍‍‍‍‌‍‍‌‌‍‍‍‍‍‌‍munity size where all members in the community know at least 3 other people**

**很长的数据流stream，找median**

Max & Small heap

**Rate Limiter**

隔离算法，两个请求之间间隔0.2s

queue 算法

token bucket: 令牌桶算法的原理是系统会以一个恒定的速度往桶里放入令牌，而如果请求需要被处理，则需要先从桶里获取一个令牌，当桶里没有令牌可取时，则拒绝服务

种是按照 token bucket 的说明，真的做放 token 的操作：

1. 后台有个线程每 1/n 秒将 bucket 中的 token 数量加一，直至达到 bucket 容量。
2. 主线程检查限速时，比较 bucket 中 token 的数量，如果少于需要的数量，表示当前被限制。 （比如，一个请求进来，检查 bucket 中的 token 数量是否 &gt; 1，如果 &gt; 1请求放行同时把 token 数量减 1， 如果 &lt; 1 说明当前请求已超出速率限制，请求被拒绝。）

每个 token bucket 都会有一个繁忙的后台线程在更新 token 数量, 会 导致严重占用系统 CPU 出现严重的性能问题

计算上次取跟这次取之间按照速率产生多少token，加上上次剩余，比较剩余

**1145. Binary Tree Coloring Game**

**Virus Source: find root**

Indegree = 0

**STAR method**

Situation/Task: context in which you took action

Action: the actions you did in response to the situation and how they did it

Results: the changes, consequences, and differences the actions made

1. planning and organizing

            establishing clear and realistic objectives

            scheduling activities and time parameters to get the job done

            setting priorities

            knowing resources needed and making the best use of it

            monitoring progress and adjusting activity

2. decision making

            gathering necessary information & facts

            using this information to work out possible courses of action to take

considering alternative courses of action

considering the implications and consequences of different courses of action

carrying out the most appropriate course of action

3. problem solving

            finding and gathering all the relevant information from the right sources

            organizing and sorting the information to identify the reasons for the problem

            coming up with possible solutions to the proble

4. adaptability

            adjusting your behavior, communication style and your approach to match changing tasks, work demands or different people

adjusting priorities to meet new deadlines and information

adjusting activities and attitude to work effectively in a new environment

willing to try new approaches for changed situations

attempting to understand and embrace change positively

5. initiative

able to be proactive and seek out new opportunities

able to capitalize on opportunities and come up with new ideas

able to solve problems without being asked

able to come up with new ways to apply existing information and knowledge

able to anticipate problems and challenges rather than just reacting to them

able to work independently and who is willing to look for ways to improve oneself and one's work environment

6. team work

            how you exchange information freely and openly

            how you are able to listen to and acknowledge the input of others

your use of empathy in dealing with team members

willing to ask for and encourage feedback and help

able to support team actions and decisions and put the team objectives ahead of your own goals

7. work standards

            imposing standards of excellence on oneself

not being satisfied with average performance

assuming responsibility and accountability for one's own successful performance and work outputs

8. communication

            the ability to listen with empathy and respect

            able to avoid interrupting and willing to hear the person out

            receiving the right message by asking appropriate questions and clarifying details

            expressing oneself effectively and clearly

using the appropriate language and communication style to match the individual you are communicating with

9. creativity

The interviewer wants an actual example from your past.

**3 DON’TS**

1. use term like usually or sometimes rather than stick to the facts

2. provide your opinion rather than a real example

3. saying what you would, in theory rather than what you actually did

General tips for resources:

            Listen carefully

            Be concise

            Highlight your strength

            Don’t worry about giving the right answer

            Come prepared with thoughtful questions

[https://www.best-job-interview.com/answers-to-behavioral-interview-questions.html](https://www.best-job-interview.com/answers-to-behavioral-interview-questions.html)

**Behaviour Questions:**

Googleyness:

            how you work individually and on a team

            how you help others

            how you navigate ambiguity

            how you push yourself to grow outside of your comfort zone

Your objectives

            reasons to choose those particular goals

            any measure you set up to track progress

            obstacles you overcome and things learnt along the way

Tell me about a time you failed to meet a deadline

            what did you fail to do

            what did you learn

Imagine you are in charge of organizing the grand opening of a new Google o       ce in

Bangalore, India. What steps would you take to plan this event?

The objective of the event, and measurement of success.

Who will be invited to the event.

Logistics around the event, set-up, location, timing.

Stakeholders to involve in the process.

**Leadership:**

How you have used your communication and decision-making skills to mobilize others

为什么选择我们公司

是什么吸引你来应聘这个岗位

你希望通过该职位获得什么

说一个你做过影响最大的project

有没有同时很多task要完成的经历

遇到和自己工作方式不同的同事会怎么处理

有没有lead过project，最大的Challenge是什么

如果组里的productivity很低可以怎么改进

怎么确保project及时deliver

怎么学习和customer相处

**Prepare Questions about Google**

