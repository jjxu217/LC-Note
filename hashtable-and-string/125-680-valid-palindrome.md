# 125/680 Valid Palindrome

## 125. Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.

**Example 1:**

```text
Input: "A man, a plan, a canal: Panama"
Output: true
```

**Example 2:**

```text
Input: "race a car"
Output: false
```

### Sol: 判断alphanumeric，用 char.isalnum\(\)

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = ''.join(ch for ch in s.lower() if ch.isalnum()) 
        l, r = 0, len(s) - 1
        while l < r:
            if s[l] != s[r]:
                return False
            l += 1
            r -= 1
        return True
```

## 680. Valid Palindrome II

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

**Example 1:**  


```text
Input: "aba"
Output: True
```

**Example 2:**  


```text
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1
        while l < r:
            if s[l] != s[r]:
                return self.isPal(s, l+1, r) or self.isPal(s, l, r-1)
            l += 1
            r -= 1
        return True
    
    def isPal(self, s, l, r):
        while l < r:
            if s[l] != s[r]:
                return False
            l += 1
            r -= 1
        return True
```

