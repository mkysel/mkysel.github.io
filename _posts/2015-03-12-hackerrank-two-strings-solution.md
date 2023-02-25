---
title: "HackerRank 'Two Strings' Solution"
date: "2015-03-12"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given two strings, A and B. Find if there is a substring that appears in both A and B.

##### Link

[Two Strings](https://www.hackerrank.com/challenges/two-strings)

##### Complexity:

time complexity is O(N+M);

space complexity is O(1)

##### Execution:

At first sight this seems like a longest common substring problem. It is actually much easier. You just need to find out if there are two equal _letters_ in both strings A and B.

##### Solution:

```python

#!/usr/bin/py
def twoStrings(s1, s2):
    m1 = set(s1)
    m2 = set(s2)
    if set.intersection(m1,m2):
        return "YES"
    return "NO"

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        first = raw_input()
        second = raw_input()
        print twoStrings(first, second)
        
```
