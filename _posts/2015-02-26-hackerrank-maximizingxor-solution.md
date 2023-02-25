---
title: "HackerRank 'Maximizing XOR' Solution"
date: "2015-02-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given two integers, _L_ and _R_, find the maximal values of _A_ xor _B_, where A and B satisfies the following condition:

- L≤A≤B≤R

##### Link

[Maximizing XOR](https://www.hackerrank.com/challenges/maximizing-xor)

##### Complexity:

time complexity is O(N^2);

space complexity is O(1)

##### Execution:

Based on the constraints, you can search by using brute force.

##### Solution:

```python

#!/usr/bin/py
def  maxXor( l,  r):
  max_xor = 0
  for low in xrange(l ,r+1):
    for high in xrange(low, r+1):
        max_xor = max(max_xor, low ^ high)
  return max_xor  

if __name__ == '__main__':
    l = int(raw_input());
    r = int(raw_input());

    res = maxXor(l, r);
    print(res)
```
