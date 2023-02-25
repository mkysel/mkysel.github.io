---
title: "HackerRank 'Mini-Max Sum' Solution"
date: "2019-05-12"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Given five positive integers, find the minimum and maximum values that can be calculated by summing exactly four of the five integers. Then print the respective minimum and maximum values as a single line of two space-separated long integers.

##### Link

[Mini-Max Sum](https://www.hackerrank.com/challenges/mini-max-sum/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Rather than recalculating the sum every time, keep track of the minimal element and maximal element. The result is the overall sum minus the min/max element.

Note: The editorial claims that the task is O(1), but I disagree. It is O(N).

##### Solution:

```python
#!/bin/python

import math
import os
import random
import re
import sys

# Complete the miniMaxSum function below.
def miniMaxSum(arr):
    min_ele = sys.maxint
    max_ele = 0
    arr_sum = 0

    for val in arr:
        min_ele = min(min_ele, val)
        max_ele = max(max_ele, val)
        arr_sum += val
    
    return arr_sum-max_ele, arr_sum-min_ele


if __name__ == '__main__':
    arr = map(int, raw_input().rstrip().split())

    print " ".join(map(str, miniMaxSum(arr)))
```
