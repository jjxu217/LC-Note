# Design

## 146. LRU Cache

Design and implement a data structure for [Least Recently Used \(LRU\) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU). It should support the following operations: `get` and `put`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a **positive** capacity.

**Follow up:**  
Could you do both operations in **O\(1\)** time complexity?

**Example:**

```text
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

### Idea: HashTable + DLL

Use case:

1. We need to store sth in the cache
2. We need to be able to add an entry to the cache and **delete the oldest entry** from the cache\(including when the cache is full\) LinkedList
3. We need to **find out quickly** whether an entry is in the cache or not HashMap{key=request, value=pointer to the ListNode}
4. We need to **adjust the priority efficiently** of each entry in the cache DoublyLinkedList

![](../.gitbook/assets/image%20%288%29.png)

```python
class DLinkedNode(): 
    def __init__(self):
        self.key = 0
        self.val = 0
        self.prev = None
        self.next = None
        
class LRUCache:
    def _add_node(self, node):
        # Always add the new node right after head.
        node.prev = self.head
        node.next = self.head.next
        
        self.head.next.prev = node
        self.head.next = node
        
    def _remove_node(self, node):
        pre, nxt = node.prev, node.next
        pre.next, nxt.prev = nxt, pre
        
    def _move_to_head(self, node):
        self._remove_node(node)
        self._add_node(node)
        
    def _pop_tail(self):
        pre = self.tail.prev
        self._remove_node(pre)
        return pre      

    def __init__(self, capacity: int):
        self.cache = {}
        self.size = 0
        self.capacity = capacity
        self.tail, self.head = DLinkedNode(), DLinkedNode()
        
        #initialize with head/tail dummy node
        self.head.next = self.tail
        self.tail.prev = self.head

    def get(self, key: int) -> int:
        node = self.cache.get(key, None)
        if not node:
            return -1        
        self._move_to_head(node)
        return node.val

    def put(self, key: int, value: int) -> None:
        node = self.cache.get(key, None)
        if not node:
            newNode = DLinkedNode()
            newNode.key, newNode.val = key, value
            
            self.cache[key] = newNode
            self._add_node(newNode)
            self.size += 1
            
            if self.size > self.capacity:
                tail = self._pop_tail()
                del self.cache[tail.key]
                self.size -= 1
        else:
            # update the value.
            node.val = value
            self._move_to_head(node)
```

## 460. LFU Cache

Design and implement a data structure for [Least Frequently Used \(LFU\)](https://en.wikipedia.org/wiki/Least_frequently_used) cache. It should support the following operations: `get` and `put`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie \(i.e., two or more keys that have the same frequency\), the least **recently** used key would be evicted.

**Follow up:**  
Could you do both operations in **O\(1\)** time complexity?

**Example:**

```text
LFUCache cache = new LFUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.get(3);       // returns 3.
cache.put(4, 4);    // evicts key 1.
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

### sol: 2 dic + DLL

### What is my data structure?

#### 1. A Doubly linked Node

```text
class Node:
	+ key: int
	+ value: int
	+ freq: int
	+ prev: Node
	+ next: Node
```

#### 2. A Doubly Linked List

```text
class DLinkedList:
	- sentinel: Node
	+ size: int
	+ append(node: Node) -> None
	+ pop(node: Node) -> Node
```

#### 3. Our LFUCache

```text
class LFUCache:
	- node: dict[key: int, node: Node]
	- freq: dict[freq: int, lst: DlinkedList]
	- minfreq: int
	+ get(key: int) -> int
	+ put(key: int, value: int) -> None
```

### Visualization

![image](https://assets.leetcode.com/users/k_kkkyle/image_1545365613.png)

### Explanation

Each key is mapping to the corresponding node \(`self._node`\), where we can retrieve the node in `O(1)` time.

Each frequency `freq` is mapped to a Doubly Linked List \(`self._freq`\), where all nodes in the `DLinkedList` have the same frequency, `freq`. Moreover, each node will be always inserted in the head \(indicating most recently used\).

A minimum frequency `self._minfreq` is maintained to keep track of the minimum frequency of across all nodes in this cache, such that the DLinkedList with the min frequency can always be retrieved in O\(1\) time.

### Here is how the algorithm works

**get\(key\)**

1. query the `node` by calling `self._node[key]`
2. find the frequency by checking `node.freq`, assigned as `f`, and query the `DLinkedList` that this node is in, through calling `self._freq[f]`
3. pop this node
4. update node's frequence, append the node to the new `DLinkedList` with frequency `f+1`
5. if the `DLinkedList` is empty and `self._minfreq == f`, update `self._minfreq` to `f+1`.
6. return `node.val`

**put\(key, value\)**

* If key is already in cache, do the same thing as `get(key)`, and update `node.val` as `value`
* Otherwise:
  1. if the cache is full, pop the least frequenly used element \(\*\)
  2. add new node to `self._node`
  3. add new node to `self._freq[1]`
  4. reset `self._minfreq` to 1

\(\*\) The least frequently used element is the **tail element in the DLinkedList with frequency self.\_minfreq**

```python
import collections
class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.freq = 1
        self.prev = self.next = None
        
class DLinkedList:
    def __init__(self):
        self.head = Node(None, None) # dummy node
        self.head.next = self.head.prev = self.head
        self._size = 0
        
    def __len__(self):
        return self._size
    
    def append(self, node):
        node.next, node.prev = self.head.next, self.head
        self.head.next = node.next.prev = node
        self._size += 1
    
    def pop(self, node=None):
        if self._size == 0:
            return
        if not node:
            node = self.head.prev
            
        node.prev.next = node.next
        node.next.prev = node.prev
        self._size -= 1
        
        return node
        
        
class LFUCache:

    def __init__(self, capacity: int):
        """
        Three things to maintain:
        
        1. a dict, named as `self._node`, for the reference of all nodes given key.
           That is, O(1) time to retrieve node given a key.
           
        2. Each frequency has a doubly linked list, store in `self._freq`, where key
           is the frequency, and value is an object of `DLinkedList`
        
        3. The min frequency through all nodes. We can maintain this in O(1) time, taking
           advantage of the fact that the frequency can only increment by 1. Use the following
		   two rules:
           
           Rule 1: Whenever we see the size of the DLinkedList of current min frequency is 0,
                   the min frequency must increment by 1.
           
           Rule 2: Whenever put in a new (key, value), the min frequency must 1 (the new node)
         """
        self._size = 0
        self._capacity = capacity
        
        self._node = dict() # key: Node
        self._freq = collections.defaultdict(DLinkedList)
        self._minfreq = 0

     
    def _update(self, node):
        """ 
        This is a helper function that used in the following two cases:     
            1. when `get(key)` is called; and
            2. when `put(key, value)` is called and the key exists.
         
        The common point of these two cases is that:        
            1. no new node comes in, and
            2. the node is visited one more times -> node.freq changed -> 
               thus the place of this node will change
        
        The logic of this function is:       
            1. pop the node from the old DLinkedList (with freq `f`)
            2. append the node to new DLinkedList (with freq `f+1`)
            3. if old DlinkedList has size 0 and self._minfreq is `f`,
               update self._minfreq to `f+1`
        
        All of the above opeartions took O(1) time.
        """
        freq = node.freq
        
        self._freq[freq].pop(node)
        if self._minfreq == freq and not self._freq[freq]:
            self._minfreq += 1
        
        node.freq += 1
        freq = node.freq
        self._freq[freq].append(node)
    
    def get(self, key):
        """
        Through checking self._node[key], we can get the node in O(1) time.
        Just performs self._update, then we can return the value of node.
        
        :type key: int
        :rtype: int
        """
        if key not in self._node:
            return -1
        
        node = self._node[key]
        self._update(node)
        return node.val

    def put(self, key, value):
        """
        If `key` already exists in self._node, we do the same operations as `get`, except
        updating the node.val to new value.
        
        Otherwise, the following logic will be performed
        
        1. if the cache reaches its capacity, pop the least frequently used item. (*)
        2. add new node to self._node
        3. add new node to the DLinkedList with frequency 1
        4. reset self._minfreq to 1
        
        (*) How to pop the least frequently used item? Two facts:
        
        1. we maintain the self._minfreq, the minimum possible frequency in cache.
        2. All cache with the same frequency are stored as a DLinkedList, with
           recently used order (Always append at head)
          
        Consequence? ==> The tail of the DLinkedList with self._minfreq is the least
                         recently used one, pop it...
        
        :type key: int
        :type value: int
        :rtype: void
        """
        if self._capacity == 0:
            return
        
        if key in self._node:
            node = self._node[key]
            self._update(node)
            node.val = value
        else:
            if self._size == self._capacity:
                node = self._freq[self._minfreq].pop()
                del self._node[node.key]
                self._size -= 1
                
            node = Node(key, value)
            self._node[key] = node
            self._freq[1].append(node)
            self._minfreq = 1
            self._size += 1
```

## 588. Design In-Memory File System

Design an in-memory file system to simulate the following functions:

`ls`: Given a path in string format. If it is a file path, return a list that only contains this file's name. If it is a directory path, return the list of file and directory names **in this directory**. Your output \(file and directory names together\) should in **lexicographic order**.

`mkdir`: Given a **directory path** that does not exist, you should make a new directory according to the path. If the middle directories in the path don't exist either, you should create them as well. This function has void return type.

`addContentToFile`: Given a **file path** and **file content** in string format. If the file doesn't exist, you need to create that file containing given content. If the file already exists, you need to **append** given content to original content. This function has void return type.

`readContentFromFile`: Given a **file path**, return its **content** in string format.

**Example:**

```text
Input: 
["FileSystem","ls","mkdir","addContentToFile","ls","readContentFromFile"]
[[],["/"],["/a/b/c"],["/a/b/c/d","hello"],["/"],["/a/b/c/d"]]

Output:
[null,[],null,null,["a"],"hello"]

Explanation:
```

```text

```

## 604. Design Compressed String Iterator

Design and implement a data structure for a compressed string iterator. It should support the following operations: `next` and `hasNext`.

The given compressed string will be in the form of each letter followed by a positive integer representing the number of this letter existing in the original uncompressed string.

`next()` - if the original string still has uncompressed characters, return the next letter; Otherwise return a white space.  
`hasNext()` - Judge whether there is any letter needs to be uncompressed.

**Note:**  
Please remember to **RESET** your class variables declared in StringIterator, as static/class variables are **persisted across multiple test cases**. Please see [here](https://leetcode.com/faq/#different-output) for more details.

**Example:**

```text
StringIterator iterator = new StringIterator("L1e2t1C1o1d1e1");

iterator.next(); // return 'L'
iterator.next(); // return 'e'
iterator.next(); // return 'e'
iterator.next(); // return 't'
iterator.next(); // return 'C'
iterator.next(); // return 'o'
iterator.next(); // return 'd'
iterator.hasNext(); // return true
iterator.next(); // return 'e'
iterator.hasNext(); // return false
iterator.next(); // return ' '
```

```python
import re
class StringIterator:
    """
    built [('L', 1), ('e', 2), ('t', 1), ('c', 1), ('o', 1), ('d', 1), ('e', 1)], then reverse
    """
    
    def __init__(self, compressedString):
        self.tokens = []
        for token in re.findall('\D\d+', compressedString):
            self.tokens.append((token[0], int(token[1:])))
        self.tokens = self.tokens[::-1]

    def next(self):
        if not self.tokens: return ' '
        t, n = self.tokens.pop()
        if n > 1: 
            self.tokens.append((t, n - 1))
        return t

    def hasNext(self):
        return bool(self.tokens)
```

## 707. Design Linked List

Design your implementation of the linked list. You can choose to use the singly linked list or the doubly linked list. A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node. If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement these functions in your linked list class:

* get\(index\) : Get the value of the `index`-th node in the linked list. If the index is invalid, return `-1`.
* addAtHead\(val\) : Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
* addAtTail\(val\) : Append a node of value `val` to the last element of the linked list.
* addAtIndex\(index, val\) : Add a node of value `val` before the `index`-th node in the linked list. If `index` equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. If index is negative, the node will be inserted at the head of the list.
* deleteAtIndex\(index\) : Delete the `index`-th node in the linked list, if the index is valid.

**Example:**

```text
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1, 2);  // linked list becomes 1->2->3
linkedList.get(1);            // returns 2
linkedList.deleteAtIndex(1);  // now the linked list is 1->3
linkedList.get(1);            // returns 3
```

```text

```

## 641. Design Circular Deque

Design your implementation of the circular double-ended queue \(deque\).

Your implementation should support following operations:

* `MyCircularDeque(k)`: Constructor, set the size of the deque to be k.
* `insertFront()`: Adds an item at the front of Deque. Return true if the operation is successful.
* `insertLast()`: Adds an item at the rear of Deque. Return true if the operation is successful.
* `deleteFront()`: Deletes an item from the front of Deque. Return true if the operation is successful.
* `deleteLast()`: Deletes an item from the rear of Deque. Return true if the operation is successful.
* `getFront()`: Gets the front item from the Deque. If the deque is empty, return -1.
* `getRear()`: Gets the last item from Deque. If the deque is empty, return -1.
* `isEmpty()`: Checks whether Deque is empty or not. 
* `isFull()`: Checks whether Deque is full or not.

**Example:**

```text
MyCircularDeque circularDeque = new MycircularDeque(3); // set the size to be 3
circularDeque.insertLast(1);			// return true
circularDeque.insertLast(2);			// return true
circularDeque.insertFront(3);			// return true
circularDeque.insertFront(4);			// return false, the queue is full
circularDeque.getRear();  			// return 2
circularDeque.isFull();				// return true
circularDeque.deleteLast();			// return true
circularDeque.insertFront(4);			// return true
circularDeque.getFront();			// return 4
```

```text

```

## 622. Design Circular Queue

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO \(First In First Out\) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Your implementation should support following operations:

* `MyCircularQueue(k)`: Constructor, set the size of the queue to be k.
* `Front`: Get the front item from the queue. If the queue is empty, return -1.
* `Rear`: Get the last item from the queue. If the queue is empty, return -1.
* `enQueue(value)`: Insert an element into the circular queue. Return true if the operation is successful.
* `deQueue()`: Delete an element from the circular queue. Return true if the operation is successful.
* `isEmpty()`: Checks whether the circular queue is empty or not.
* `isFull()`: Checks whether the circular queue is full or not.

**Example:**

```text
MyCircularQueue circularQueue = new MyCircularQueue(3); // set the size to be 3
circularQueue.enQueue(1);  // return true
circularQueue.enQueue(2);  // return true
circularQueue.enQueue(3);  // return true
circularQueue.enQueue(4);  // return false, the queue is full
circularQueue.Rear();  // return 3
circularQueue.isFull();  // return true
circularQueue.deQueue();  // return true
circularQueue.enQueue(4);  // return true
circularQueue.Rear();  // return 4
```

```text

```

