---
title: "Codility 'AbsDistinct' Solution"
date: "2014-08-14"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute number of distinct absolute values of sorted array elements.

##### Link

[AbsDistinct](https://codility.com/demo/take-sample-test/abs_distinct)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Additional storage is allowed. Therefore a simple python solution will suffice.

##### Solution:

```python

def solution(A):
    return len(set([abs(x) for x in A]))
```
