---
title: "Codility 'MinMaxDivision' Solution"
date: "2014-09-16"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Divide array A into K blocks and minimize the largest sum of any block.

##### Link

[MinMaxDivision](https://codility.com/demo/take-sample-test/min_max_division)

##### Complexity:

expected worst-case time complexity is O(N\*log(N+M));

expected worst-case space complexity is O(1)

##### Execution:

Binary search for the minimal size of a block. A valid block can be checked in a boolean fashion. Two special cases can be sped up (courtesy to [CodeSays](http://codesays.com/2014/solution-to-min-max-division-by-codility/)). **At the time of writing (19.9.2014) do not use the variable passed to the solution function as M! It is NOT the maximum element in the test cases! The specification says, that no element is larger than M, yet there is not guarantee that M == max(A).**

##### Solution:

```python

def blockSizeIsValid(A, max_block_cnt, max_block_size):
    block_sum = 0
    block_cnt = 0

    for element in A:
        if block_sum + element > max_block_size:
            block_sum = element
            block_cnt += 1
        else:
            block_sum += element
        if block_cnt >= max_block_cnt:
            return False

    return True

def binarySearch(A, max_block_cnt, using_M_will_give_you_wrong_results):
    lower_bound = max(A)
    upper_bound = sum(A)

    if max_block_cnt == 1:      return upper_bound
    if max_block_cnt >= len(A): return lower_bound

    while lower_bound <= upper_bound:
        candidate_mid = (lower_bound + upper_bound) // 2
        if blockSizeIsValid(A, max_block_cnt, candidate_mid):
            upper_bound = candidate_mid - 1
        else:
            lower_bound = candidate_mid + 1

    return lower_bound

def solution(K, M, A):
    return binarySearch(A,K,M)
```
