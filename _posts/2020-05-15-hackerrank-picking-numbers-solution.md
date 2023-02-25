---
title: "HackerRank 'Picking Numbers' Solution"
date: "2020-05-15"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given an array of integers, find and print the maximum number of integers you can select from the array such that the absolute difference between any two of the chosen integers is less than or equal to 1.

##### Link

[Picking Numbers](https://www.hackerrank.com/challenges/picking-numbers/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

Calculate the occurrence of every element. The largest subset is the sum of two adjacent elements. For simplicity, I calculate the result in the same loop. It could be split out.

##### Solution:

```python
from collections import defaultdict

def pickingNumbers(a):
    d = defaultdict(int)
    r_val = 0
    for val in a:
        d[val] += 1
        r_val = max(r_val, d[val]+d[val+1], d[val]+d[val-1])

    return r_val
```
