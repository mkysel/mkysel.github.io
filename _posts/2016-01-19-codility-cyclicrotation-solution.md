---
title: "Codility 'CyclicRotation' Solution"
date: "2016-01-19"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Rotate an array to the right by a given number of steps.

##### Link

[Cyclic Rotation](https://codility.com/programmers/task/cyclic_rotation/)

##### Complexity:

expected worst-case time complexity is O(N)

##### Execution:

There are multiple solutions to this problem. I picked the one that does not create a copy of the array.

##### Solution:

```python

def reverse(arr, i, j):
    for idx in xrange((j - i + 1) / 2):
        arr[i+idx], arr[j-idx] = arr[j-idx], arr[i+idx]

def solution(A, K):
    l = len(A)
    if l == 0:
        return []
        
    K = K%l
    
    reverse(A, l - K, l -1)
    reverse(A, 0, l - K -1)
    reverse(A, 0, l - 1)

    return A
```
