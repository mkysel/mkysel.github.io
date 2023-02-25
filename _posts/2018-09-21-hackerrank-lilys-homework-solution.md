---
title: "HackerRank 'Lily's Homework' Solution"
date: "2018-09-21"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Whenever George asks Lily to hang out, she's busy doing homework. George wants to help her finish it faster, but he's in over his head! Can you help George understand Lily's homework so she can hang out with him?

Consider an array of m distinct integers, arr = \[a\[0\], a\[1\], ..., a\[n-1\]\]. George can swap any two elements of the array any number of times. An array is _beautiful_ if the sum of |a\[i\] - a\[i-1\] among 0 < i < n is minimal.

##### Link

[Lily's Homework](https://www.hackerrank.com/challenges/lilys-homework)

##### Complexity:

time complexity is O(N\*log(N))

space complexity is O(N)

##### Execution:

Let us rephrase the problem to a sorting problem: Find the number of swaps to make the array sorted. We need to consider both ascending and descending arrays. The solution assumes no duplicates.

##### Solution:

```python

def cntSwaps(arr):
    positions = sorted(list(enumerate(arr)), key=lambda e: e[1])
    swaps = 0
    
    for idx in xrange(len(arr)):
        while True:
            if (positions[idx][0] == idx):
                break
            else:
                swaps += 1
                swapped_idx = positions[idx][0]
                positions[idx], positions[swapped_idx] = positions[swapped_idx], positions[idx]
    
    return swaps

def lilysHomework(arr):
    return min(cntSwaps(arr), cntSwaps(arr[::-1]))
```
