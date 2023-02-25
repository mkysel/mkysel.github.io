---
title: "HackerRank 'Pairs' Solution"
date: "2015-08-31"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given N integers, count the number of pairs of integers whose difference is K.

##### Link

[Pairs](https://www.hackerrank.com/challenges/pairs)

##### Complexity:

time complexity is O(N\*log(N))

space complexity is O(N)

##### Execution:

The solution is pretty straight-forward, just read the code :). The runtime complexity is calculated with log(N) access times for tree-based sets (not the case in Python).

Note: The problem specification has a contraint on K: 0<K<10^9. If K was allowed to be 0, this challenge would become slightly more difficult.

##### Solution:

```python

#!/usr/bin/py
def pairs(a,k):
    answer = 0
    s = set(a)
    for v in s:
        if v+k in s:
            answer += 1

    return answer
if __name__ == '__main__':
    n, k = map(int, raw_input().split())
    b = map(int, raw_input().split())
    print pairs(b, k)
```
