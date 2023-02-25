---
title: "Codility 'FrogRiverOne' Solution"
date: "2014-05-20"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the earliest time when a frog can jump to the other side of a river.

##### Link

[FrogRiverOne](https://codility.com/demo/take-sample-test/frog_river_one)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(X)`

##### Execution:

Mark seen elements as such in a boolean array. I do not like the idea of returning the first second as 0. But specifications are specifications :)

##### Solution:

```python

def solution(X, A):
    passable = [False] * X
    uncovered = X

    for idx in xrange(len(A)):
        if A[idx] <= 0 or A[idx] > X:
            raise Exception("Invalid value", A[idx])
        if passable[A[idx]-1] == False:
            passable[A[idx]-1] = True
            uncovered -= 1
            if uncovered == 0:
                return idx

    return -1
```
