---
title: "Codility 'CountDistinctSlices' Solution"
date: "2014-08-15"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count the number of distinct slices (containing only unique numbers).

##### Link

[CountDistinctSlices](https://codility.com/demo/take-sample-test/count_distinct_slices)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(M)

##### Execution:

Using the caterpillar method I expand the caterpillar to the right as long as a duplicate element is found. The right side has to retract as long as this duplicate element has been eliminated from the next slice. An observation showed that the number of sub-slices is equal to front-back+1.

##### Solution:

```python

def solution(M, A):
    the_sum = 0
    front = back = 0
    seen = [False] * (M+1)
    while (front < len(A) and back < len(A)):
        while (front < len(A) and seen[A[front]] != True):
            the_sum += (front-back+1)
            seen[A[front]] = True
            front += 1
        else:
            while front < len(A) and back < len(A) and A[back] != A[front]:
                seen[A[back]] = False
                back += 1
                
            seen[A[back]] = False
            back += 1
            
                
    return min(the_sum, 1000000000)  
```
