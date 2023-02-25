---
title: "Codility 'NumberSolitaire' Solution"
date: "2014-12-23"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

In a given array, find the subset of maximal sum in which the distance between consecutive elements is at most 6.

##### Link

[NumberSolitaire](https://codility.com/demo/take-sample-test/number_solitaire)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Prototypical Dynamic Programming. Remember all sub-solutions and use them to compute the next step. I prefixed the array by 6 min\_values to use the same computation for the whole algorithm (besides the first). Do not forget to set the minimal\_value small enough to handle a purely negative input array.

##### Solution:

```python

NR_POSSIBLE_ROLLS = 6
MIN_VALUE = -10000000001

def solution(A):
    sub_solutions = [MIN_VALUE] * (len(A)+NR_POSSIBLE_ROLLS)
    sub_solutions[NR_POSSIBLE_ROLLS] = A[0]
    
    # iterate over all steps
    for idx in xrange(NR_POSSIBLE_ROLLS+1, len(A)+NR_POSSIBLE_ROLLS):
        max_previous = MIN_VALUE
        for previous_idx in xrange(NR_POSSIBLE_ROLLS):
            max_previous = max(max_previous, sub_solutions[idx-previous_idx-1])
        # the value for each iteration is the value at the A array plus the best value from which this index can be reached
        sub_solutions[idx] = A[idx-NR_POSSIBLE_ROLLS] + max_previous
    
    return sub_solutions[len(A)+NR_POSSIBLE_ROLLS-1]
```
