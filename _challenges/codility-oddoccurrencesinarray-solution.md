---
title: "Codility 'OddOccurrencesInArray' Solution"
date: "2014-12-11"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find value that occurs in odd number of elements.

##### Link

[OddOccurrencesInArray](https://codility.com/demo/take-sample-test/odd_occurrences_in_array "Odd Occurrences In Array")

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(1)`

##### Execution:

This problem can be found in many algorithm books. A xor A cancels itself and B xor 0 is B. Therefore A xor A xor B xor C xor C is B.

##### Solution:

```python

def solution(A):
    missing_int = 0
    for value in A:
        missing_int ^= value
    return missing_int
```
