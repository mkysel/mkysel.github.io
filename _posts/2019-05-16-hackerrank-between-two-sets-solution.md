---
title: "HackerRank 'Between Two Sets' Solution"
date: "2019-05-16"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You will be given two arrays of integers and asked to determine all integers that satisfy the following two conditions:

1. The elements of the first array are all factors of the integer being considered
2. The integer being considered is a factor of all elements of the second array

##### Link

[Between Two Sets](https://www.hackerrank.com/challenges/between-two-sets/problem)

##### Complexity:

time complexity is O(A\* (N+M))

space complexity is O(1)

##### Execution:

This challenge could also be solved using the _Greatest Common Divisor_. Given that the range of values is only \[1,100\], it is safe to assume that the _naive_ solution will terminate within the time limit.

##### Solution:

```python
#!/bin/python

import sys

def isValid(a, b, candidate):
    for a_ele in a:
        if candidate % a_ele != 0:
            return False
    for b_ele in b:
        if b_ele % candidate != 0:
            return False
    return True

n,m = raw_input().strip().split(' ')
n,m = [int(n),int(m)]
a = map(int,raw_input().strip().split(' '))
b = map(int,raw_input().strip().split(' '))

cnt = 0
for candidate in xrange(max(a), min(b)+1):
    if isValid(a, b, candidate):
        cnt += 1

print cnt
```
