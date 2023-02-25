---
title: "Codility 'Count Factors' Solution"
date: "2014-07-28"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count factors of given number N.

##### Link

[CountFactors](https://codility.com/demo/take-sample-test/count_factors)

##### Complexity:

expected worst-case time complexity is `O(sqrt(N))`

expected worst-case space complexity is `O(1).`

##### Execution:

This example can be found in the lesson document.

##### Solution:

```python

def solution(N):
    cnt = 0
    i = 1
    while ( i * i <= N):
        if (N % i == 0):
            if i * i == N:
               cnt += 1
            else:
                cnt += 2
        i += 1
    return cnt
```
