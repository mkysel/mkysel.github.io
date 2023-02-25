---
title: "Codility 'MinAvgTwoSlice' Solution"
date: "2015-01-02"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the minimal average of any slice containing at least two elements.

##### Link

[MinAvgTwoSlice](https://codility.com/demo/take-sample-test/min_avg_two_slice)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Every slice must be of size two or three. Slices of bigger sizes are created from such smaller slices. Therefore should any bigger slice have an optimal value, all sub-slices must be the same, for this case to hold true. Should this not be true, one of the sub-slices must be the optimal slice. The others being bigger. Therefore we check all possible slices of size 2/3 and return the smallest one. The first such slice is the correct one, do not use <=!

For other languages use BigInts or equal. This line _(A\[idx\] + A\[idx+1\] + A\[idx+2\])/3.0_ can easily overflow!

You can read the formal proof by Minh Tran Dao [here](https://github.com/daotranminh/playground/blob/master/src/codibility/MinAvgTwoSlice/proof.pdf).

##### Solution:

```python

def solution(A):
    min_idx = 0
    min_value = 10001

    for idx in xrange(0, len(A)-1):
        if (A[idx] + A[idx+1])/2.0 < min_value:
            min_idx = idx
            min_value = (A[idx] + A[idx+1])/2.0
        if idx < len(A)-2 and (A[idx] + A[idx+1] + A[idx+2])/3.0 < min_value:
            min_idx = idx
            min_value = (A[idx] + A[idx+1] + A[idx+2])/3.0

    return min_idx
```
