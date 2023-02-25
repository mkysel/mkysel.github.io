---
title: "Codility 'MaxSliceSum' Solution"
date: "2015-01-06"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find a maximum sum of a compact subsequence of array elements.

##### Link

[MaxSliceSum](https://codility.com/demo/take-sample-test/max_slice_sum)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(N)`

##### Execution:

The only difference to the example given by Codility is the minimal slice length, which is 1.

##### Solution:

```python

def solution(A):
    max_ending = max_slice = -1000000
    for a in A:
        max_ending = max(a, max_ending +a)
        max_slice = max(max_slice, max_ending)
        
    return max_slice
```
