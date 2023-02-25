---
title: "HackerRank 'Super Reduced String' Solution"
date: "2018-09-12"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Steve has a string of lowercase characters in range `ascii[‘a’..’z’]`. He wants to reduce the string to its shortest length by doing a series of operations. In each operation he selects a pair of adjacent lowercase letters that match, and he deletes them. For instance, the string `aab` could be shortened to `b` in one operation.

Steve’s task is to delete as many characters as possible using this method and print the resulting string. If the final string is empty, print `Empty String`

##### Link

[Super Reduced String](https://www.hackerrank.com/challenges/reduced-string)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

The solution creates a stack of values. If the top value on the stack is equivalent to the next value, simply remove both. This solution assumes that there are only ever 2 values next to each other. If any adjacent values were to be removed, I would require more loops.

##### Solution:

```python

i = raw_input()

s = []

for c in i:
    if not s:
        s.append(c)
    else:
        if s[-1] == c:
            s.pop()
        else:
            s.append(c)
            
if not s:
    print "Empty String"
else:
    print ''.join(s)
```
