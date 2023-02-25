---
title: "HackerRank 'Circle City' Solution"
date: "2015-04-08"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Roy lives in a city that is circular in shape on a 2D plane that has radius r. The city center is located at origin (0,0) and it has suburbs lying on the lattice points (points with integer coordinates). The city Police Department Headquarters can only protect those suburbs which are located strictly inside the city. The suburbs located on the border of the city are still unprotected. So the police department decides to build at most k additional police stations at some suburbs. Each of these police stations can protect the suburb it is located in.

##### Link

[Circle City](https://www.hackerrank.com/challenges/circle-city)

##### Complexity:

time complexity is `O(sqrt(N))`

space complexity is `O(1)`

##### Execution:

You have to solve the circle equation _M^2 + N^2 = D_ where M and N are integral numbers. There is an article on [wiki](http://en.wikipedia.org/wiki/Gauss_circle_problem "Gauss Circle Problem").

##### Solution:

```python

#!/usr/bin/py
from math import sqrt, ceil

def isCircleProtected(d, K):
    count = 0
    for m in xrange(int(ceil(sqrt(d)))):
        if sqrt(d - m*m).is_integer():
            count += 4
    
    return count <= K

if __name__ == '__main__':
    t = input()
    for _ in range(t):
    	d, k = map(int, raw_input().split())
        if (isCircleProtected(d, k)):
            print "possible"
        else:
            print "impossible"

```
