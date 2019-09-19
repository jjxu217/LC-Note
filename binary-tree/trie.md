# Trie

#### 1.1 什么是Trie树 <a id="11-&#x4EC0;&#x4E48;&#x662F;trie&#x6811;"></a>

Trie树，即字典树，又称单词查找树或键树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：最大限度地减少无谓的字符串比较，查询效率比哈希表高。

Trie的核心思想是空间换时间。利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

它有3个基本性质：

* 根节点不包含字符，除根节点外每一个节点都只包含一个字符。
* 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。
* 每个节点的所有子节点包含的字符都不相同。

#### 1.2 树的构建 <a id="12-&#x6811;&#x7684;&#x6784;&#x5EFA;"></a>

举个在网上流传颇广的例子，如下：

题目：给你100000个长度不超过10的单词。对于每一个单词，我们要判断他出没出现过，如果出现了，求第一次出现在第几个位置。

分析：这题当然可以用hash来解决，但是本文重点介绍的是trie树，因为在某些方面它的用途更大。比如说对于某一个单词，我们要询问它的前缀是否出现过。这样hash就不好搞了，而用trie还是很简单。

现在回到例子中，如果我们用最傻的方法，对于每一个单词，我们都要去查找它前面的单词中是否有它。那么这个算法的复杂度就是O\(n^2\)。显然对于100000的范围难以接受。现在我们换个思路想。假设我要查询的单词是abcd，那么在他前面的单词中，以b，c，d，f之类开头的我显然不必考虑。而只要找以a开头的中是否存在abcd就可以了。同样的，在以a开头中的单词中，我们只要考虑以b作为第二个字母的，一次次缩小范围和提高针对性，这样一个树的模型就渐渐清晰了。

好比假设有b，abc，abd，bcd，abcd，efg，hii 这6个单词，我们构建的树就是如下图这样的：

![](https://mnmunknown.gitbooks.io/algorithm-notes/trie_1.jpg)

当时第一次看到这幅图的时候，便立马感到此树之不凡构造了。单单从上幅图便可窥知一二，好比大海搜人，立马就能确定东南西北中的到底哪个方位，如此迅速缩小查找的范围和提高查找的针对性，不失为一创举。

ok，如上图所示，对于每一个节点，从根遍历到他的过程就是一个单词，如果这个节点被标记为红色，就表示这个单词存在，否则不存在。

那么，对于一个单词，我只要顺着他从根走到对应的节点，再看这个节点是否被标记为红色就可以知道它是否出现过了。把这个节点标记为红色，就相当于插入了这个单词。

这样一来我们查询和插入可以一起完成（重点体会这个查询和插入是如何一起完成的，稍后，下文具体解释），所用时间仅仅为单词长度，在这一个样例，便是10。

我们可以看到，trie树每一层的节点数是26^i级别的。所以为了节省空间。我们用动态链表，或者用数组来模拟动态。空间的花费，不会超过单词数×单词长度。

#### 1.3 前缀查询 <a id="13-&#x524D;&#x7F00;&#x67E5;&#x8BE2;"></a>

上文中提到”比如说对于某一个单词，我们要询问它的前缀是否出现过。这样hash就不好搞了，而用trie还是很简单“。下面，咱们来看看这个前缀查询问题：

已知n个由小写字母构成的平均长度为10的单词,判断其中是否存在某个串为另一个串的前缀子串。下面对比3种方法：

* 最容易想到的：即从字符串集中从头往后搜，看每个字符串是否为字符串集中某个字符串的前缀，复杂度为O\(n^2\)。
* 使用hash：我们用hash存下所有字符串的所有的前缀子串，建立存有子串hash的复杂度为O\(n_len\)，而查询的复杂度为O\(n\)_ O\(1\)= O\(n\)。
* 使用trie：因为当查询如字符串abc是否为某个字符串的前缀时，显然以b,c,d....等不是以a开头的字符串就不用查找了。所以建立trie的复杂度为O\(n_len\)，而建立+查询在trie中是可以同时执行的，建立的过程也就可以成为查询的过程，hash就不能实现这个功能。所以总的复杂度为O\(n_len\)，实际查询的复杂度也只是O\(len\)。（说白了，就是Trie树的平均高度h为len，所以Trie树的查询复杂度为O（h）=O（len）。好比一棵二叉平衡树的高度为logN，则其查询，插入的平均时间复杂度亦为O（logN））。

下面解释下上述方法3中所说的为什么hash不能将建立与查询同时执行，而Trie树却可以：

在hash中，例如现在要输入两个串911，911456，如果要同时查询这两个串，且查询串的同时若hash中没有则存入。那么，这个查询与建立的过程就是先查询其中一个串911，没有，然后存入9、91、911；而后查询第二个串911456，没有然后存入9、91、911、9114、91145、911456。因为程序没有记忆功能，所以并不知道911在输入数据中出现过，只是照常以例行事，存入9、91、911、9114、911...。也就是说用hash必须先存入所有子串，然后for循环查询。

而trie树中，存入911后，已经记录911为出现的字符串，在存入911456的过程中就能发现而输出答案；倒过来亦可以，先存入911456，在存入911时，当指针指向最后一个1时，程序会发现这个1已经存在，说明911必定是某个字符串的前缀。

读者反馈@悠悠长风：关于这点，我有不同的看法。hash也是可以实现边建立边查询的啊。当插入911时，需要一个额外的标志位，表示它是一个完整的单词。在处理911456时，也是按照前面的查询9,91,911，当查询911时，是可以找到前面插入的911，且通过标志位知道911为一个完整单词。那么就可以判断出911为911456的前缀啊。虽然trie树更适合这个问题，但是我认为hash也是可以实现边建立，边查找。

至于，有关Trie树的查找，插入等操作的实现代码，网上遍地开花且千篇一律，诸君尽可参考，想必不用我再做多余费神。

#### 1.4 查询 <a id="14-&#x67E5;&#x8BE2;"></a>

Trie树是简单但实用的数据结构，通常用于实现字典查询。我们做即时响应用户输入的AJAX搜索框时，就是Trie开始。本质上，Trie是一颗存储多个字符串的树。相邻节点间的边代表一个字符，这样树的每条分支代表一则子串，而树的叶节点则代表完整的字符串。和普通树不同的地方是，相同的字符串前缀共享同一条分支。下面，再举一个例子。给出一组单词，inn, int, at, age, adv, ant, 我们可以得到下面的Trie：

![](https://mnmunknown.gitbooks.io/algorithm-notes/trie_2.gif)

可以看出：

* 每条边对应一个字母。
* 每个节点对应一项前缀。叶节点对应最长前缀，即单词本身。
* 单词inn与单词int有共同的前缀“in”, 因此他们共享左边的一条分支，root-&gt;i-&gt;in。同理，ate, age, adv, 和ant共享前缀"a"，所以他们共享从根节点到节点"a"的边。
* 查询操纵非常简单。比如要查找int，顺着路径i -&gt; in -&gt; int就找到了。

搭建Trie的基本算法也很简单，无非是逐一把每则单词的每个字母插入Trie。插入前先看前缀是否存在。如果存在，就共享，否则创建对应的节点和边。比如要插入单词add，就有下面几步：

* 考察前缀"a"，发现边a已经存在。于是顺着边a走到节点a。
* 考察剩下的字符串"dd"的前缀"d"，发现从节点a出发，已经有边d存在。于是顺着边d走到节点ad
* 考察最后一个字符"d"，这下从节点ad出发没有边d了，于是创建节点ad的子节点add，并把边ad-&gt;add标记为d。

#### 1.5 Trie树的应用 <a id="15-trie&#x6811;&#x7684;&#x5E94;&#x7528;"></a>

除了本文引言处所述的问题能应用Trie树解决之外，Trie树还能解决下述问题（节选自此文：海量数据处理面试题集锦与Bit-map详解）：

3、有一个1G大小的一个文件，里面每一行是一个词，词的大小不超过16字节，内存限制大小是1M。返回频数最高的100个词。+

9、1000万字符串，其中有些是重复的，需要把重复的全部去掉，保留没有重复的字符串。请怎么设计和实现？

10、 一个文本文件，大约有一万行，每行一个词，要求统计出其中最频繁出现的前10个词，请给出思想，给出时间复杂度分析。

13、寻找热门查询： 搜索引擎会通过日志文件把用户每次检索使用的所有检索串都记录下来，每个查询串的长度为1-255字节。假设目前有一千万个记录，这些查询串的重复读比较高，虽然总数是1千万，但是如果去除重复和，不超过3百万个。一个查询串的重复度越高，说明查询它的用户越多，也就越热门。请你统计最热门的10个查询串，要求使用的内存不能超过1G。

\(1\) 请描述你解决这个问题的思路；

\(2\) 请给出主要的处理流程，算法，以及算法的复杂度。

## 211. Add and Search Word - Data structure design

Design a data structure that supports the following two operations:

```text
void addWord(word)
bool search(word)
```

search\(word\) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.

**Example:**

```text
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

### Sol: use dict to represent trie, key is child 

```python
class WordDictionary:
    def __init__(self):
        self.trie = {}

    def addWord(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None

    def search(self, word: str) -> bool:
        def find(word, node):
            if not word:
                return None in node         
            char, word = word[0], word[1:]
            if char == '.':
                return any(find(word, kid) for kid in node.values() if kid)     
            return char in node and find(word, node[char])
            
        return find(word, self.trie)
```

### sol: use TrieNode

```python
class TrieNode():
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.isWord = False
    
class WordDictionary(object):
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word):
        node = self.root
        for w in word:
            node = node.children[w]
        node.isWord = True

    def search(self, word):
        def find(word, node):
            if not word:
                return node.isWord
            char, word = word[0], word[1:]
            if char == '.':
                return any(find(word, kid) for kid in node.children.values())
            return char in node.children.keys() and find(word, node.children[char])
    
        return find(word, self.root)
```

## 208. Implement Trie \(Prefix Tree\)

Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```text
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

```python
class Trie:
    def __init__(self):
        self.trie = {}       

    def insert(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None      

    def search(self, word: str) -> bool:
        def find(word, node):
            if not word:
                return None in node         
            char, word = word[0], word[1:]
            return char in node and find(word, node[char])
            
        return find(word, self.trie)

    def startsWith(self, prefix: str) -> bool:
        def helper(prefix, node):
            if not prefix: 
                return True
            char, prefix = prefix[0], prefix[1:]
            return char in node and helper(prefix, node[char])
        
        return helper(prefix, self.trie)
```

## 642. Design Search Autocomplete System

Design a search autocomplete system for a search engine. Users may input a sentence \(at least one word and end with a special character `'#'`\). For **each character** they type **except '\#'**, you need to return the **top 3** historical hot sentences that have prefix the same as the part of sentence already typed. Here are the specific rules:

1. The hot degree for a sentence is defined as the number of times a user typed the exactly same sentence before.
2. The returned top 3 hot sentences should be sorted by hot degree \(The first is the hottest one\). If several sentences have the same degree of hot, you need to use ASCII-code order \(smaller one appears first\).
3. If less than 3 hot sentences exist, then just return as many as you can.
4. When the input is a special character, it means the sentence ends, and in this case, you need to return an empty list.

Your job is to implement the following functions:

The constructor function:

`AutocompleteSystem(String[] sentences, int[] times):` This is the constructor. The input is **historical data**. `Sentences` is a string array consists of previously typed sentences. `Times` is the corresponding times a sentence has been typed. Your system should record these historical data.

Now, the user wants to input a new sentence. The following function will provide the next character the user types:

`List<String> input(char c):` The input `c` is the next character typed by the user. The character will only be lower-case letters \(`'a'` to `'z'`\), blank space \(`' '`\) or a special character \(`'#'`\). Also, the previously typed sentence should be recorded in your system. The output will be the **top 3** historical hot sentences that have prefix the same as the part of sentence already typed. 

**Example:**  
**Operation:** AutocompleteSystem\(\["i love you", "island","ironman", "i love leetcode"\], \[5,3,2,2\]\)  
The system have already tracked down the following sentences and their corresponding times:  
`"i love you"` : `5` times  
`"island"` : `3` times  
`"ironman"` : `2` times  
`"i love leetcode"` : `2` times  
Now, the user begins another search:  
  
**Operation:** input\('i'\)  
**Output:** \["i love you", "island","i love leetcode"\]  
**Explanation:**  
There are four sentences that have prefix `"i"`. Among them, "ironman" and "i love leetcode" have same hot degree. Since `' '` has ASCII code 32 and `'r'` has ASCII code 114, "i love leetcode" should be in front of "ironman". Also we only need to output top 3 hot sentences, so "ironman" will be ignored.  
  
**Operation:** input\(' '\)  
**Output:** \["i love you","i love leetcode"\]  
**Explanation:**  
There are only two sentences that have prefix `"i "`.  
  
**Operation:** input\('a'\)  
**Output:** \[\]  
**Explanation:**  
There are no sentences that have prefix `"i a"`.  
  
**Operation:** input\('\#'\)  
**Output:** \[\]  
**Explanation:**  
The user finished the input, the sentence `"i a"` should be saved as a historical sentence in system. And the following input will be counted as a new search.

```python
class TrieNode():
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.isEnd = False
        self.data = None
        self.rank = 0
        
class Trie():
    def __init__(self):
        self.root = TrieNode()
        
    def addRecord(self, sentence, hot):
        node = self.root
        for c in sentence:
            node = node.children[c]
        node.isEnd = True
        node.data = sentence
        node.rank -= hot
        
    def dfs(self, node):
        res = []
        if node.isEnd:
            res.append((node.rank, node.data))
        for child in node.children.values():
            res.extend(self.dfs(child))
        return res
        
    def search(self, sentence):
        node = self.root
        for c in sentence:
            if c not in node.children:
                return []
            node = node.children[c]
        return self.dfs(node)
        
class AutocompleteSystem():
    def __init__(self, sentences, times):
        self.t = Trie()
        self.keyword = ""
        for i, sentence in enumerate(sentences):
            self.t.addRecord(sentence, times[i])   
    
    def input(self, c):
        results = []
        if c != "#":
            self.keyword += c
            results = self.t.search(self.keyword)
        else:
            self.t.addRecord(self.keyword, 1)
            self.keyword = ""
        return [item[1] for item in sorted(results)[:3]]

# Your AutocompleteSystem object will be instantiated and called as such:
# obj = AutocompleteSystem(sentences, times)
# param_1 = obj.input(c)
```

## 212. Word Search II

{% page-ref page="../graph/dfs/79-212-word-search.md" %}

## 745. Prefix and Suffix Search

Given many `words`, `words[i]` has weight `i`.

Design a class `WordFilter` that supports one function, `WordFilter.f(String prefix, String suffix)`. It will return the word with given `prefix` and `suffix` with maximum weight. If no word exists, return -1.

**Examples:**

```text
Input:
WordFilter(["apple"])
WordFilter.f("a", "e") // returns 0
WordFilter.f("b", "") // returns -1
```

## 1065. Index Pairs of a String

Given a `text` string and `words` \(a list of strings\), return all index pairs `[i, j]` so that the substring `text[i]...text[j]` is in the list of `words`.

**Example 1:**

```text
Input: text = "thestoryofleetcodeandme", words = ["story","fleet","leetcode"]
Output: [[3,7],[9,13],[10,17]]
```

**Example 2:**

```text
Input: text = "ababa", words = ["aba","ab"]
Output: [[0,1],[0,2],[2,3],[2,4]]
Explanation: 
Notice that matches can overlap, see "aba" is found in [0,2] and [2,4].
```

```python
class Trie:
    def __init__(self):
        self.trie = {}       

    def insert(self, word: str) -> None:
        node = self.trie
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None      

    
class Solution:
    def indexPairs(self, text: str, words: List[str]) -> List[List[int]]:
        t = Trie()
        res = []
        for word in words:
            t.insert(word)
        for l in range(len(text)):
            #for every iteration, search from the beginning
            node = t.trie                 
            for r in range(l, len(text)):
                if text[r] not in node:
                    break
                node = node[text[r]]
                if None in node: #if find a valid word
                    res.append([l, r])
        return res
```

## 720. Longest Word in Dictionary

Given a list of strings `words` representing an English Dictionary, find the longest word in `words` that can be built one character at a time by other words in `words`. If there is more than one possible answer, return the longest word with the smallest lexicographical order.If there is no answer, return the empty string.

**Example 1:**  


```text
Input: 
words = ["w","wo","wor","worl", "world"]
Output: "world"
Explanation: 
The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".
```

**Example 2:**  


```text
Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".
```

### Trie + DFS

```python
class Trie:
    def __init__(self):
        self.root = {}       

    def insert(self, word: str) -> None:
        node = self.root
        for w in word:
            node = node.setdefault(w, {})
        node[None] = None  
        
    def dfs(self):
        self.res = ''
        def helper(node, pre):
            for c, child in node.items():
                if c and None in child:
                    cand = pre + c
                    helper(child, cand)
                    if len(cand) > len(self.res) or (len(cand) == len(self.res) and cand < self.res):
                        self.res = cand
        helper(self.root, '')    
        return self.res
        
class Solution:
    def longestWord(self, words: List[str]) -> str:
        t = Trie()
        for word in words:
            t.insert(word)
        return t.dfs()          
```

```python
def longestWord(self, words):
        valid = set([""])       
        for word in sorted(words, key=lambda x: len(x)):
           if word[:-1] in valid:
                valid.add(word)								
        return max(sorted(valid), key=lambda x: len(x))
```

