---
title: "HackerRank 'Left Rotation' Solution"
date: "2019-05-20"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

A _left rotation_ operation on an array shifts each of the array's elements _1_ unit to the left. For example, if _2_ left rotations are performed on array _\[1,2,3,4,5\]_, then the array would become _\[3,4,5,1,2\]_.

##### Link

[Arrays: Left Rotation](https://www.hackerrank.com/challenges/ctci-array-left-rotation)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

Solutions like this is where python really shines. Simple and straight forward.

##### Solution:

```python
#!/bin/python

import math
import os
import random
import re
import sys

# Complete the rotLeft function below.
def rotLeft(a, d):
    return a[d:] + a[:d]

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    nd = raw_input().split()

    n = int(nd[0])

    d = int(nd[1])

    a = map(int, raw_input().rstrip().split())

    result = rotLeft(a, d)

    fptr.write(' '.join(map(str, result)))
    fptr.write('\n')

    fptr.close()
```
