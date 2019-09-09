# 243/244/245 Shortest Word Distance

## 243. Shortest Word Distance

Given a list of words and two words _word1_ and _word2_, return the shortest distance between these two words in the list.

**Example:**  
Assume that words = `["practice", "makes", "perfect", "coding", "makes"]`.

```text
Input: word1 = “coding”, word2 = “practice”
Output: 3
```

```text
Input: word1 = "makes", word2 = "coding"
Output: 1
```

### Sol: one pass

```python
class Solution:
    def shortestDistance(self, words: List[str], word1: str, word2: str) -> int:
        min_dist = len(words)
        curr_word, idx = None, 0
        for i, w in enumerate(words):
            if w not in (word1, word2): continue
            if curr_word and w != curr_word:
                min_dist = min(min_dist, i - idx)
            curr_word, idx = w, i
        return min_dist
```

## 244. Shortest Word Distance II

Design a class which receives a list of words in the constructor, and implements a method that takes two words _word1_ and _word2_ and return the shortest distance between these two words in the list. Your method will be called _repeatedly_ many times with different parameters. 

**Example:**  
Assume that words = `["practice", "makes", "perfect", "coding", "makes"]`.

```text
Input: word1 = “coding”, word2 = “practice”
Output: 3
```

```text
Input: word1 = "makes", word2 = "coding"
Output: 1
```

### Sol: build a dict, store {ele: index\(sorted\)}

```python
class WordDistance:
    def __init__(self, words: List[str]):
        self.l = collections.defaultdict(list)
        for i, w in enumerate(words):
            self.l[w].append(i)

    def shortest(self, word1: str, word2: str) -> int:
        loc1, loc2 = self.l[word1], self.l[word2]
        p1 = p2 = 0
        dist = float('inf')
        while p1 < len(loc1) and p2 < len(loc2):
            dist = min(dist, abs(loc1[p1] - loc2[p2]))
            if loc1[p1] < loc2[p2]:
                p1 += 1
            else:
                p2 += 1
        return dist
```

## 245. Shortest Word Distance III

Given a list of words and two words _word1_ and _word2_, return the shortest distance between these two words in the list.

_**word1**_ **and** _**word2**_ **may be the same and they represent two individual words in the list.**

**Example:**  
Assume that words = `["practice", "makes", "perfect", "coding", "makes"]`.

```text
Input: word1 = “makes”, word2 = “coding”
Output: 1
```

```text
Input: word1 = "makes", word2 = "makes"
Output: 3
```

### Sol: one pass

```python
class Solution:
    def shortestWordDistance(self, words: List[str], word1: str, word2: str) -> int:
        min_dist = len(words)
        curr_word, idx = None, 0
        for i, w in enumerate(words):
            if w not in (word1, word2): continue
            if curr_word and (w != curr_word or word1 == word2):
                min_dist = min(min_dist, i - idx)
            curr_word, idx = w, i
        return min_dist
```

