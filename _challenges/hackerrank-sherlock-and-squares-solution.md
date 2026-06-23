---
title: "HackerRank 'Sherlock and Squares' Solution"
date: "2015-03-05"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Watson gives two integers A & B to Sherlock and asks if he can count the number of square integers between A and B (both inclusive).

A square integer is an integer which is the square of any integer. For example, 1, 4, 9, 16 are some of the square integers as they are squares of 1, 2, 3, 4 respectively.

##### Link

[Sherlock and Squares](https://www.hackerrank.com/challenges/sherlock-and-squares)

##### Complexity:

time complexity is `O(sqrt(N))`

space complexity is `O(1)`

##### Execution:

Just compute the difference between the square of the low end and the high end.

##### Solution:

```python

#!/usr/bin/py
from math import *

if __name__ == '__main__':
    t = input()
    for _ in range(t):
        a, b = map(int, raw_input().split())
        a = ceil(sqrt(a))
        b = floor(sqrt(b))
        print int(b - a) + 1
```
