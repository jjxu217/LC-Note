# 删除

#### 涉及链表删除操作的时候，稳妥起见都用 dummy node，省去很多麻烦。因为不一定什么时候 head 就被删了。 <a id="&#x6D89;&#x53CA;&#x94FE;&#x8868;&#x5220;&#x9664;&#x64CD;&#x4F5C;&#x7684;&#x65F6;&#x5019;&#xFF0C;&#x7A33;&#x59A5;&#x8D77;&#x89C1;&#x90FD;&#x7528;-dummy-node&#xFF0C;&#x7701;&#x53BB;&#x5F88;&#x591A;&#x9EBB;&#x70E6;&#x3002;&#x56E0;&#x4E3A;&#x4E0D;&#x4E00;&#x5B9A;&#x4EC0;&#x4E48;&#x65F6;&#x5019;-head-&#x5C31;&#x88AB;&#x5220;&#x4E86;&#x3002;"></a>

## 19. Remove Nth Node From End of List

Given a linked list, remove the _n_-th node from the end of list and return its head.

**Example:**

```text
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given _n_ will always be valid.

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        right = left = dummy = ListNode(0)
        dummy.next = head
        for i in range(n):
            right = right.next
        while right.next:
            left = left.next
            right = right.next
        left.next = left.next.next
        return dummy.next
```

## 237. Delete Node in a Linked List

Write a function to delete a node \(except the tail\) in a singly linked list, given only access to that node.

Given linked list -- head = \[4,5,1,9\], which looks like following:

![](https://assets.leetcode.com/uploads/2018/12/28/237_example.png)

**Example 1:**

```text
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

**Example 2:**

```text
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
```

```python
class Solution:
    def deleteNode(self, node):
        pre = None
        while node.next:
            node.val =  node.next.val
            pre = node
            node = node.next
        pre.next = None
```

## 203. Remove Linked List Elements

Remove all elements from a linked list of integers that have value **val**.

**Example:**

```text
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

```python
class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        pre = dummy = ListNode(0)
        dummy.next = cur = head
        while cur:
            if cur.val == val:
                pre.next = cur.next
                cur = cur.next
            else:
                cur = cur.next
                pre = pre.next
        return dummy.next
```

