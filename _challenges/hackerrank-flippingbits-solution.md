---
title: "HackerRank 'Flipping Bits' Solution"
date: "2015-02-24"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You will be given a list of 32 bits unsigned integers. You are required to output the list of the unsigned integers you get by flipping bits in its binary representation (i.e. unset bits must be set, and set bits must be unset).

##### Link

[Flipping Bits](https://www.hackerrank.com/challenges/flipping-bits)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

It can be done by either using the negation ~ operator, or by XORing the value with 2^32 -1 (all 1).

##### Solution:

```python

#!/usr/bin/py
def flipBits(a):
   return a ^ 4294967295 # 2^32 - 1

if __name__ == '__main__':
    n = int(raw_input())
    for i in range(0,n):
    	a = int(raw_input())
    	res = flipBits(a)
    	print res
```
