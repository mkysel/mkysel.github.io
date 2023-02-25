---
title: "Codility 'Peaks' Solution"
date: "2014-07-31"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

A non-empty zero-indexed array A consisting of N integers is given. A peak is an array element which is larger than its neighbors.

##### Link

[Peaks](https://codility.com/demo/take-sample-test/peaks)

##### Complexity:

expected worst-case time complexity is `O(N\*log(log(N)))`

expected worst-case space complexity is `O(N)`

##### Execution:

I first compute all peaks. Because each block must contain a peak I start from the end and try to find a integral divisor sized block. If each block contains a peak I return the size.

##### Solution:

```python


def solution(A):
    peaks = []

    for idx in xrange(1, len(A)-1):
        if A[idx-1] < A[idx] > A[idx+1]:
            peaks.append(idx)

    if len(peaks) == 0:
        return 0

    for size in xrange(len(peaks), 0, -1):
        if len(A) % size == 0:
            block_size = len(A) // size
            found = [False] * size
            found_cnt = 0
            for peak in peaks:
                block_nr = peak//block_size
                if found[block_nr] == False:
                    found[block_nr] = True
                    found_cnt += 1

            if found_cnt == size:
                return size

    return 0

```
