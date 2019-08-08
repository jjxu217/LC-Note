# Binary Search



### **How do we identify Binary Search?**

As mentioned in earlier, Binary Search is an algorithm that _divides the search space in 2_ after every comparison. Binary Search should be considered every time you need to search for an index or element in a collection. If the collection is unordered, we can always sort it first before applying Binary Search.

### **3 Parts of a Successful Binary Search**

Binary Search is generally composed of 3 main sections:

1. _**Pre-processing**_ - Sort if collection is unsorted.
2. _**Binary Search**_ - Using a loop or recursion to divide search space in half after each comparison.
3. _**Post-processing**_ - Determine viable candidates in the remaining space.



### Binary Search Template Analysis

**Template Explanation:**

99% of binary search problems that you see online will fall into 1 of these 3 templates. Some problems can be implemented using multiple templates, but as you practice more, you will notice that some templates are more suited for certain problems than others.

**Note:** The templates and their differences have been colored coded below.

![](https://leetcode.com/explore/learn/card/binary-search/136/template-analysis/Figures/binary_search/Template_Diagram.png)

These 3 templates differ by their:

* left, mid, right index assignments
* loop or recursive termination condition
* necessity of post-processing

Template 1 and 3 are the most commonly used and almost all binary search problems can be easily implemented in one of them. Template 2 is a bit more advanced and used for certain types of problems.

Each of these 3 provided templates provide a specific use case:

**Template \#1** `(left <= right)`:

* Most basic and elementary form of Binary Search
* Search Condition can be determined without comparing to the element's neighbors \(or use specific elements around it\)
* No post-processing required because at each step, you are checking to see if the element has been found. If you reach the end, then you know the element is not found

**Template \#2** `(left < right)`:

* An advanced way to implement Binary Search.
* Search Condition needs to access element's immediate right neighbor
* Use element's right neighbor to determine if condition is met and decide whether to go left or right
* Gurantees Search Space is at least 2 in size at each step
* Post-processing required. Loop/Recursion ends when you have 1 element left. Need to assess if the remaining element meets the condition.

**Template \#3** `(left + 1 < right):`

* An alternative way to implement Binary Search
* Search Condition needs to access element's immediate left and right neighbors
* Use element's neighbors to determine if condition is met and decide whether to go left or right
* Gurantees Search Space is at least 3 in size at each step
* Post-processing required. Loop/Recursion ends when you have 2 elements left. Need to assess if the remaining elements meet the condition.

