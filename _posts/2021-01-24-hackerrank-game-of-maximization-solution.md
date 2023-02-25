---
title: "HackerRank 'Game of Maximization' Solution"
date: "2021-01-24"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "hackerrank-hackfest-2020"
  - "python"
---

##### Short Problem Definition:

There are n piles of stones, where the ith pile has ai stones. You need to collect the maximum number of stones from these piles

##### Link

[Stones Problem](https://www.hackerrank.com/contests/hackerrank-hackfest-2020/challenges/stones-piles/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

This challenge is fairly simple. It does not matter how those stones are arranged. The only important bit is that either the sum of all even elements is bigger than the sum of all odd elements or vice versa.

##### Solution:

```python
def maximumStones(arr):
    return 2*min(sum(arr[0::2]), sum(arr[1::2]))
```
