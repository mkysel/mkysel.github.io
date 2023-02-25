---
title: "Codility 'MaxSliceSum' Solution"
date: "2014-12-29"
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

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Can be solved by slightly adapting the golden\_max\_slice from the tutorial, not allowing empty slices.

##### Solution:

```python

def golden_max_slice(A):
    max_ending = max_slice = -1000000
    for a in A:
        max_ending = max(a, max_ending +a)
        max_slice = max(max_slice, max_ending)
        
    return max_slice

def solution(A):
    return golden_max_slice(A)
```
