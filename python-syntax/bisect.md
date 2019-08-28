# bisect

{% embed url="https://www.geeksforgeeks.org/bisect-algorithm-functions-in-python/" %}

### bisect.bisect\_left\(_list_, _item_\[, _lo_\[, _hi_\]\]\)

This function returns the **position** in the **sorted** list, where the number passed in argument can be placed so as to **maintain the resultant list in sorted order**. If the element is already present in the list, the **left most position** where element has to be inserted is returned. **This function takes 4 arguments, list which has to be worked with, number to insert, starting position in list to consider, ending position which has to be considered**.

### bisect.bisect\_right\(_list_, _item_\[, _lo_\[, _hi_\]\]\) = bisect.bisect\(\)

Find the right most position.

### bisect.insort\_left\(_list_, _item_\[, _lo_\[, _hi_\]\]\)

Insert _item_ in _list_ in sorted order. This is equivalent to list.insert\(bisect.bisect\_left\(list, item, lo, hi\), item\). This assumes that _list_ is already sorted.

### bisect.insort\_right\(_list_, _item_\[, _lo_\[, _hi_\]\]\) = bisect.insort\(_..._\)

### Note: lo is contain, hi is not

