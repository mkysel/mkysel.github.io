---
title: "Codility 'MaxProductOfThree' Solution"
date: "2014-08-06"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Maximize A\[P\] \* A\[Q\] \* A\[R\] for any triplet (P, Q, R).

##### Link

[MaxProductOfThree](https://codility.com/demo/take-sample-test/max_product_of_three)

##### Complexity:

expected worst-case time complexity is O(N\*log(N));

expected worst-case space complexity is O(1)

##### Execution:

After sorting the largest product can be found as a combination of the last three elements. Additionally, two negative numbers add to a positive, so by multiplying the two largest negatives with the largest positive, we get another candidate. If all numbers are negative, the three largest (closest to 0) still get the largest element!

##### Solution:

```python

def solution(A):
    if len(A) < 3:
        raise Exception("Invalid input")
        
    A.sort()
    
    return max(A[0] * A[1] * A[-1], A[-1] * A[-2] * A[-3])
```

A better solution has been suggested in the commends by [Mindaugas Tvaronavičius](https://disqus.com/by/mindaugastvaronaviius/). Here the solution in O(N) time using two heaps.

```python

def betterSolution(A):
    if len(A) < 3:
        raise Exception("Invalid input")
        
    minH = []
    maxH = []
    
    for val in A:
        if len(minH) < 2:
            heapq.heappush(minH, -val)
        else:
            heapq.heappushpop(minH, -val)
            
        if len(maxH) < 3:
            heapq.heappush(maxH, val)
        else:
            heapq.heappushpop(maxH, val)
    
    
    max_val = heapq.heappop(maxH) * heapq.heappop(maxH)
    top_ele = heapq.heappop(maxH)
    max_val *= top_ele
    min_val = -heapq.heappop(minH) * -heapq.heappop(minH) * top_ele
    
    return max(max_val, min_val)
```
