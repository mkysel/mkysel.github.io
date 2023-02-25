---
title: "HackerRank 'Time Complexity: Primality' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

A _prime_ is a natural number _greater than_ 1 that has no positive divisors other than 1 and itself. Given n integers, determine the primality of each integer and print whether it is `Prime` or `Not prime` on a new line.

**Note:** If possible, try to come up with an O(sqrt(N)) primality algorithm, or see what sort of optimizations you can come up with for an O(N) algorithm.

##### Link

[Time Complexity: Primality](https://www.hackerrank.com/challenges/ctci-big-o/problem)

##### Complexity:

time complexity is O(sqrt(N))

space complexity is O(1)

##### Execution:

The point of this question is to make sure that you only iterate up to (including) sqrt(N).

##### Solution:

```python
def primality(n):
    if n == 1:
        return False

    for i in xrange(2, int(math.sqrt(n))+1):
        if n % i == 0:
            return False
    return True
```
