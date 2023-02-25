---
title: "Codility 'MinAbsSumOfTwo' Solution"
date: "2014-08-12"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the minimal absolute value of a sum of two elements.

##### Link

[MinAbsSumOfTwo](https://codility.com/demo/take-sample-test/min_abs_sum_of_two)

##### Complexity:

expected worst-case time complexity is `O(N\*log(N))`

expected worst-case space complexity is `O(1)`

##### Execution:

Using the caterpillar method on a sorted list.

##### Solution:

C-style python

```python

def solution(A):
    value = 2000000000
    front_ptr = 0
    back_ptr = len(A)-1
    A.sort()
    
    while front_ptr <= back_ptr: value = min(value, abs(A[front_ptr] + A[back_ptr])) if abs(A[front_ptr]) > abs(A[back_ptr]):
            front_ptr += 1
        else:
            back_ptr -= 1
            
    return value
```

Functional pythonesque:

```python

from itertools import *
def getAbsDiff(t):
  return abs(t[0] + t[1])

def solution(A):
  A.sort(key=abs)
  return getAbsDiff(min(chain(izip(A, A),izip(A,A[1:])), key = getAbsDiff))
```
