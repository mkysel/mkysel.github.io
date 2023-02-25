---
title: "HackerRank 'Balanced Parentheses' Solution"
date: "2016-01-25"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given a sequence consisting of parentheses, determine whether the expression is balanced.

##### Link

[Balanced Parentheses](https://www.hackerrank.com/challenges/balanced-parentheses)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

Equivalent to [Codility Brackets](http://www.martinkysel.com/codility-brackets-solution/).

##### Solution:

```python

def isValidPair(left, right):
    if left == '(' and right == ')':
        return True
    if left == '[' and right == ']':
        return True 
    if left == '{' and right == '}':
        return True   
    return False
 
def isProperlyNested(S):
    stack = []
     
    for symbol in S:
        if symbol == '[' or symbol == '{' or symbol == '(':
            stack.append(symbol)
        else:
            if len(stack) == 0:
                return False
            last = stack.pop()
            if not isValidPair(last, symbol):
                return False
     
    if len(stack) != 0:
        return False
             
    return True

def main():
    N = int(raw_input())

    for _ in xrange(N):
        s = raw_input()
        if isProperlyNested(s):
            print "YES"
        else:
            print "NO"


if __name__ == '__main__':
    main()
```
