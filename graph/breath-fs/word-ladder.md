# 126/127 Word Ladder

## 127. Word Ladder

Given two words \(_beginWord_ and _endWord_\), and a dictionary's word list, find the length of shortest transformation sequence from _beginWord_ to _endWord_, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.

**Example 1:**

```text
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

### Solution: BFS, use deque，记录下一个word和已经跳的次数

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        wordList = set(wordList)
        queue = collections.deque([[beginWord, 1]])
        while queue:
            word, length = queue.popleft()
            if word == endWord:
                return length
            for i in range(len(word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    next_word = word[:i] + c + word[i+1:]
                    if next_word in wordList:
                        wordList.remove(next_word)
                        queue.append([next_word, length + 1])
        return 0
```

## 126. Word Ladder II

Return the path

**Example 1:**

```text
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

**Example 2:**

```text
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: []

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

### Sol: BFS, traverse layer by layer，dic={word, list of list=all path}

since we need to record all the possible paths。路径先分开再重合，必须layer by layer处理，删除visited的node；不可像上题那样，一旦有一个node visited ，直接删除。

```python
class Solution:
    def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:
        if endWord not in wordList or not endWord or not beginWord or not wordList:
            return []
        
        wordList = set(wordList)               
        layer = {} 
        layer[beginWord] = [[beginWord]] # stores current word and all possible paths
        while layer:
            new_layer = collections.defaultdict(list)
            for w in layer:
                if endWord == w:    
                    return layer[endWord] # return all found sequences
                else:
                    for i in range(len(w)): # change every possible letter and check if it's in dictionary
                        for c in 'abcdefghijklmnopqrstuvwxyz':
                            new_word = w[:i] + c + w[i+1:]
                            if new_word in wordList:
                            # add new word to all sequences and form new layer element
                                new_layer[new_word] += [j + [new_word] for j in layer[w]] 
            wordList -= set(new_layer.keys()) # remove from dictionary to prevent loops
            layer = new_layer
        return []
```

### 

