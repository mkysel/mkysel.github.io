---
title: "Codility 'ChocolatesByNumbers' Solution"
date: "2014-08-10"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

There are N chocolates in a circle. Count the number of chocolates you will eat.

##### Link

[ChocolatesByNumbers](https://codility.com/demo/take-sample-test/chocolates_by_numbers)

##### Complexity:

expected worst-case time complexity is `O(log(N+M))`

expected worst-case space complexity is `O(1)`

##### Execution:

N and M meet at their least common multiply. Dividing this LCM by M gets the number of steps(chocolates) that can be eaten.

##### Solution:

```python

def gcd(p, q):
  if q == 0:
    return p
  return gcd(q, p % q)

def lcm(p,q):
    return p * (q / gcd(p,q))

def solution(N, M):
    return lcm(N,M)/M
```

* * *

```python

def naive(N,M):
    eaten = [False] * N
    at = 0
    cnt = 0
    while eaten[at] != True:
        eaten[at] = True
        at = (at + M) % N
        cnt += 1
        
    return cnt
```
