---
title: "Codility 'MissingInteger' Solution"
date: "2014-06-01"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the minimal positive integer not occurring in a given sequence.

##### Link

[MissingInteger](https://codility.com/demo/take-sample-test/missing_integer)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

You only need to consider the first (N) positive integers. In this specification 0 does not count as a valid candidate! Any value that is below 1 or above N can be ignored.

##### Solution:

```python

def solution(A):
    seen = [False] * len(A)
    for value in A:
        if 0 < value <= len(A):
            seen[value-1] = True

    for idx in xrange(len(seen)):
        if seen[idx] == False:
            return idx + 1

    return len(A)+1
```
