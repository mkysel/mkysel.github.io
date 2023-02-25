---
title: "HackerRank 'Closest Numbers' Solution"
date: "2015-03-19"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given a list of unsorted integers, A={a1,a2,…,aN}, can you find the pair of elements that have the smallest absolute difference between them? If there are multiple pairs, find them all.

##### Link

[Closest Numbers](https://www.hackerrank.com/challenges/closest-numbers)

##### Complexity:

time complexity is `O(n\*log(n)) // sorting`

space complexity is `O(n)`

##### Execution:

Just sort the array and print the smallest difference.

##### Solution:

```python

#!/usr/bin/py
from sys import maxint

def closest(a):
    a.sort()
    smallest_difference = maxint
    smallest_pairs = []
    
    for idx in xrange(len(a)-1):
        diff = a[idx+1] - a[idx]
        if diff < smallest_difference:
            smallest_difference = diff
            smallest_pairs = [(a[idx], a[idx+1])]
        elif diff == smallest_difference:
            smallest_pairs.append((a[idx], a[idx+1]))
    
    for pair in smallest_pairs:
        print pair[0], pair[1],
    
if __name__ == '__main__':
    n = input()
    vec = map(int, raw_input().split())
    closest(vec)
```
