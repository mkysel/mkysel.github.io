---
title: "Codility 'Nesting' Solution"
date: "2014-07-25"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Determine whether given string of parentheses is properly nested.

##### Link

Nesting

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(1)`

##### Execution:

Because there is only one type of brackets, the problem is easier than [Brackets](http://www.martinkysel.com/codility-brackets-solution/ "Codility ‘Brackets’ Solution"). Just check if there is always a opening bracket before a closing one.

##### Solution:

```python

def solution(S):
    leftBrackets = 0
    
    for symbol in S:
        if symbol == '(':
            leftBrackets += 1
        else:
            if leftBrackets == 0:
                return 0
            leftBrackets -= 1  
    
    if leftBrackets != 0:
        return 0
    
    return 1
```
