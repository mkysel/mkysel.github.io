---
title: "HackerRank 'Weighted Uniform Strings' Solution"
date: "2018-09-17"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

A weighted string is a string of lowercase English letters where each letter has a _weight_. Character weights are 1 to  26 from a to z...

##### Link

[Weighted Uniform String](https://www.hackerrank.com/challenges/weighted-uniform-string)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

Parsing the string for every query is suboptimal, so I first preprocess the string. Now we know that uniform strings contain the same characters. A string can be of length 1. Do a single pass of the string and create all uniform substrings.

##### Solution:

```python

def weightedUniformStrings(s, queries):
    weights = set()
    prev = -1
    length = 0
    for c in s:
        weight = ord(c) - ord('a') + 1
        weights.add(weight)
        if prev == c:
            length += 1
            weights.add(length*weight)
        else:
            prev = c
            length = 1
    
    rval = []
    for q in queries:
        if q in weights:
            rval.append("Yes")
        else:
            rval.append("No")
    return rval
```
