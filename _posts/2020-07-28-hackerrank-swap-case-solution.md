---
title: "HackerRank 'sWAP cASE' Solution"
date: "2020-07-28"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given a string and your task is to _swap cases_. In other words, convert all lowercase letters to uppercase letters and vice versa.

##### Link

[sWAP cASE](https://www.hackerrank.com/challenges/swap-case/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

I had too much fun with this one, so I refuse to admit that there is a [buildin swapcase() function](https://python-reference.readthedocs.io/en/latest/docs/str/swapcase.html). ASCII can be fun.

##### Solution:

```python
def swap_case(s):
    result = ""
    for idx in xrange(len(s)):
        ordinal = ord(s[idx])
        if (ordinal >= ord('a') and ordinal <= ord('z')) or \
            (ordinal >= ord('A') and ordinal <= ord('Z')):
            result += chr(ordinal-ord('A')+32)%64+ord('A'))
        else:
            result += s[idx]
    return result
```
