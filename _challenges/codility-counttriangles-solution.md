---
title: "Codility 'CountTriangles' Solution"
date: "2014-09-26"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count the number of triangles that can be built from a given set of edges.

##### Link

[CountTriangles](https://codility.com/demo/take-sample-test/count_triangles)

##### Complexity:

expected worst-case time complexity is `O(N2)`

expected worst-case space complexity is `O(1)`

##### Execution:

Apply the caterpillar method. We know that in a sorted array every position between Q and R will be bigger than Q and therefore P+Q will be bigger than R. I therefore either increment Q if P+Q is not larger than R or increment R as far as possible.

##### Solution:

```python

def solution(A):
    A.sort()
    print A
    triangle_cnt = 0
    
    for P_idx in xrange(0, len(A)-2):
        Q_idx = P_idx + 1
        R_idx = P_idx + 2
        while (R_idx < len(A)):
            if A[P_idx] + A[Q_idx] > A[R_idx]:
                triangle_cnt += R_idx - Q_idx
                R_idx += 1
            elif Q_idx < R_idx -1:
                    Q_idx += 1
            else:
                R_idx += 1
                Q_idx += 1
                
    return triangle_cnt
```
