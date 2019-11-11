# \(FB\) 链表的递归与倒序打印

#### \(FB\) 在不修改链表结构和数据的情况下，倒序打印链表 <a id="fb-&#x5728;&#x4E0D;&#x4FEE;&#x6539;&#x94FE;&#x8868;&#x7ED3;&#x6784;&#x548C;&#x6570;&#x636E;&#x7684;&#x60C5;&#x51B5;&#x4E0B;&#xFF0C;&#x5012;&#x5E8F;&#x8F93;&#x51FA;&#x94FE;&#x8868;"></a>

```python
class Solution(object):
    def printReverse(head):
        if not head.next:
            print(str(head.val) + ' ')
            return
        self.printReverse(head.next)
        print(str(head.val) + ' ')
```

