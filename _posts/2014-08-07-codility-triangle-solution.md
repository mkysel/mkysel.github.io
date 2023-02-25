---
title: "Codility 'Triangle' Solution"
date: "2014-08-07"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Determine whether a triangle can be built from a given set of edges.

##### Link

[Triangle](https://codility.com/demo/take-sample-test/triangle)

##### Complexity:

expected worst-case time complexity is O(N\*log(N));

expected worst-case space complexity is O(N)

##### Execution:

By sorting the array, we have guaranteed that P+R > Q and Q+R > P (because R is always the biggest). Now what remains, is the proof that P+Q > R, that can be found out by traversing the array. The chance to find such a combination is with three adjacent values as they provide the highest P and Q.

##### Solution:

```python

def solution(A):
    if 3 > len(A):
        return 0

    A.sort()

    for i in xrange(len(A)-2):
        if A[i] + A[i+1] > A[i+2]:
            return 1

    return 0
```
