---
title: "Codility 'MaxDoubleSliceSum' Solution"
date: "2014-12-30"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the maximal sum of any double slice.

##### Link

[MaxDoubleSliceSum](https://codility.com/demo/take-sample-test/max_double_slice_sum)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

To solve this task, you need to keep track of two slice arrays. The optimal double slice can be found at an index that has the maximal sum of those two arrays. It can not be the 0th or the last index.

##### Solution:

```python

def solution(A):
    ending_here = [0] * len(A)
    starting_here = [0] * len(A)
    
    for idx in xrange(1, len(A)):
        ending_here[idx] = max(0, ending_here[idx-1] + A[idx])
    
    for idx in reversed(xrange(len(A)-1)):
        starting_here[idx] = max(0, starting_here[idx+1] + A[idx])
    
    max_double_slice = 0
    
    for idx in xrange(1, len(A)-1):
        max_double_slice = max(max_double_slice, starting_here[idx+1] + ending_here[idx-1])
        
        
    return max_double_slice
```
