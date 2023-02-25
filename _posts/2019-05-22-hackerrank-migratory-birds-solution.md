---
title: "HackerRank 'Migratory Birds' Solution"
date: "2019-05-22"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You have been asked to help study the population of birds migrating across the continent. Each type of bird you are interested in will be identified by an integer value. Each time a particular kind of bird is spotted, its id number will be added to your array of sightings. You would like to be able to find out which type of bird is most common given a list of sightings. Your task is to print the type number of that bird and if two or more types of birds are equally common, choose the type with the smallest ID number.

##### Link

[Migratory Birds](https://www.hackerrank.com/challenges/migratory-birds/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

I provide two solutions to this challenge. Solution #1 is using the Counter collection in python and works well.

The second solution is more explicit and takes advantage of the restrictive challenge specification.

##### Solution:

```python
#!/bin/python

import math
import os
import random
import re
import sys
from collections import Counter

# Complete the migratoryBirds function below.
def migratoryBirds(arr):
    mc = Counter(arr)
    return mc.most_common(1)[0][0]

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    arr_count = int(raw_input().strip())

    arr = map(int, raw_input().rstrip().split())

    result = migratoryBirds(arr)

    fptr.write(str(result) + '\n')

    fptr.close()
```

```python
# Complete the migratoryBirds function below.
def migratoryBirds(arr):
    frequencies = [0] * 6

    for ele in arr:
        frequencies[ele] += 1

    max_val = 0
    max_idx = 5
    
    for idx in xrange(5, 0, -1):
        if frequencies[idx] >= max_val:
            max_idx = idx
            max_val = frequencies[idx]

    return max_idx
```
