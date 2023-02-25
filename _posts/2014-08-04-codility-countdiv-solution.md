---
title: "Codility 'CountDiv' Solution"
date: "2014-08-04"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute number of integers divisible by k in range \[a..b\].

##### Link

[CountDiv](https://codility.com/demo/take-sample-test/count_div)

##### Complexity:

expected worst-case time complexity is O(1);

expected worst-case space complexity is O(1)

##### Execution:

This little check required a bit of experimentation. One needs to start from the first valid value that is bigger than A and a multiply of K.

##### Solution:

```python

def solution(A, B, K):
    if B < A or K <= 0:
        raise Exception("Invalid Input")

    min_value =  ((A + K -1) // K) * K

    if min_value > B:
      return 0

    return ((B - min_value) // K) + 1
```
