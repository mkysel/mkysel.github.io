---
title: "Codility 'Ladder' Solution"
date: "2014-12-27"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count the number of different ways of climbing to the top of a ladder.

##### Link

[Ladder](https://codility.com/demo/take-sample-test/ladder)

##### Complexity:

expected worst-case time complexity is O(L)

expected worst-case space complexity is O(L)

##### Execution:

We first compute the Fibonacci sequence for the first L+2 numbers. The first two numbers are used only as fillers, so we have to index the sequence as A\[idx\]+1 instead of A\[idx\]-1. The second step is to replace the modulo operation by removing all but the n lowest bits. A discussion can be found on [Stack Overflow](http://stackoverflow.com/a/6670766 "Mod of power 2").

##### Solution:

```python

def solution(A, B):
    L = max(A)
    P_max = max(B)
 
    fib = [0] * (L+2)
    fib[1] = 1
    for i in xrange(2, L + 2):
        fib[i] = (fib[i-1] + fib[i-2]) & ((1 << P_max) - 1)
 
    return_arr = [0] * len(A)
 
    for idx in xrange(len(A)):
        return_arr[idx] = fib[A[idx]+1] & ((1 << B[idx]) - 1)
 
    return return_arr
```
