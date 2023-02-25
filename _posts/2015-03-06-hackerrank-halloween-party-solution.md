---
title: "HackerRank 'Halloween Party' Solution"
date: "2015-03-06"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Alex is attending a Halloween party with his girlfriend Silvia. At the party, Silvia spots the corner of an infinite chocolate bar. If the chocolate can be served as only 1 x 1 sized pieces and Alex can cut the chocolate bar exactly _K_ times, what is the maximum number of chocolate pieces Alex can cut and give Silvia?

##### Link

[Halloween Party](https://www.hackerrank.com/challenges/halloween-party)

##### Complexity:

time complexity is O(1);

space complexity is O(1)

##### Execution:

The product a\*b (where a+b = k) is maximal, when a is close to b as possible. We can only use integral values, so a and b are rounded halves of k.

##### Solution:

```python

#!/usr/bin/py
def oneOneBars(K):
    half = K//2
    return half * (K-half)

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        k = input()
        print oneOneBars(k)
```
