---
title: "Codility 'PermMissingElem' Solution"
date: "2014-05-28"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the missing element in a given permutation.

##### Link

[PermMissingElem](https://codility.com/demo/take-sample-test/perm_missing_elem)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(1)`

##### Execution:

Sum all elements that should be in the list and sum all elements that actually are in the list. The sum is 0 based, so +1 is required. The first solution using the + operator can cause int overflow in not-python languages. Therefore the use of a binary XOR is adequate.

##### Solution:

```python

def solution(A):
    should_be = len(A) # you never see N+1 in the iteration
    sum_is = 0

    for idx in xrange(len(A)):
        sum_is += A[idx]
        should_be += idx+1

    return should_be - sum_is +1
```

* * *

```python

def solution(A):
    missing_element = len(A)+1
    
    for idx,value in enumerate(A):
        missing_element = missing_element ^ value ^ (idx+1)
        
    return missing_element
```
