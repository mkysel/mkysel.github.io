---
title: "Codility 'Str Symmetry Point' Solution"
date: "2014-12-07"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find a symmetry point of a string, if any.

##### Link

[StrSymmetryPoint](https://codility.com/demo/take-sample-test/str_symmetry_point "Str Symmetry Point")

##### Complexity:

expected worst-case time complexity is O(length(S));

expected worst-case space complexity is O(1) (not counting the storage required for input arguments).

##### Execution:

This problem gave me a lot of headache. It is so trivial I that over-complicated it. I thought that you should find a symmetry point at any possible position, ignoring the residual characters. You would obviously try to maximize the length of this symmetrical sub-array. I was not able to come with any O(S) algorithm for this problem derivation. So just to remind you, **this problem is a simple palindrome check**. Additionally, you drop all evenly sized strings as their symmetry point is between the indexes.

##### Solution:

```python

def solution(S):
    l = len(S)

    if l % 2 == 0:
        return -1

    mid_point = l // 2

    for idx in xrange(0, mid_point):
        if S[idx] != S[l - idx - 1]:
            return -1

    return mid_point
```
