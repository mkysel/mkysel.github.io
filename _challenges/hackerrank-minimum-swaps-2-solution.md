---
title: "HackerRank 'Minimum Swaps 2' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given an unordered array consisting of consecutive integers \[1, 2, 3, ..., n\] without any duplicates. You are allowed to swap any two elements. You need to find the minimum number of swaps required to sort the array in ascending order.

##### Link

[Minimum Swaps 2](https://www.hackerrank.com/challenges/minimum-swaps-2/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

This solution runs in O(N) since it will visit every element at most 2 times. No need for complex cycle algorithms, stacks, etc.

The trick is to put every element in the place it belongs to and swap it with the element at that position. It is possible that the swapped element wont be the correct once, hence the while loop.

##### Solution:

```python
def minimumSwaps(arr):
    swaps = 0
    n = len(arr)

    for idx in xrange(n):
        while arr[idx]-1 != idx:
            ele = arr[idx]
            arr[ele-1], arr[idx] = arr[idx], arr[ele-1]
            swaps += 1
    return swaps
```
