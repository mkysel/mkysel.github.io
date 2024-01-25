---
title: HackerRank 'Product Distribution' Solution
date: '2020-07-31'
categories:
  - coding-challenge
  - hack-the-interview
  - hackerrank
---

##### Short Problem Definition:

A company has requested to streamline their product allocation strategy, and given n products, each of which has an associated value, you are required to arrange these products into segments for processing. There are infinite segments indexed as 1, 2, 3 and so on.

##### Link

[Product Distribution](https://www.hackerrank.com/contests/hacktheinterview3/challenges/distribution-in-m-bins/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

There are a few things you need to keep in mind here:

- the buckets must all be the same size and can not be empty
- the last bucket can contain the remainder of the N/M elements
- sort the array so that the largest values are as far out as possible

Keep an eye out for the edge conditions.

##### Solution:

```python
MODULO = 1000000007

def maxScore(a, m):
    n = len(a)
    bucket = 1
    result = 0
    
    a.sort()
    
    i = 0
    while i + (2*m) <= n:
        result += sum(a[i:i+m])*bucket
        i += m
        bucket += 1
        
        result %= MODULO
        
    result += sum(a[i:])*bucket
    
    return result % MODULO
```
