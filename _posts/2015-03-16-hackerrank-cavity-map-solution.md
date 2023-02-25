---
title: "HackerRank 'Cavity Map' Solution"
date: "2015-03-16"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given a square map of size n×n. Each cell of the map has a value denoting its depth. We will call a cell of the map a cavity if and only if this cell is not on the border of the map and each cell adjacent to it has **strictly smaller depth**. Two cells are adjacent if they have a common side.

##### Link

[Cavity Map](https://www.hackerrank.com/challenges/cavity-map)

##### Complexity:

time complexity is O(N^2)

space complexity is O(1)

##### Execution:

\*pun\* You only get your cavities checked on an airport \*/pun\*

I check all 4 surrounding elements if they are strictly smaller. If so, I mark the position with an 'X'.  Better runtime than n^2 is not possible as every element has to be visited!

##### Solution:

```python

#!/usr/bin/py

def transformCavity(arr, n):
    for idx_tb in xrange(1, n-1):
        for idx_lr in xrange(1, n-1):
            if arr[idx_tb-1][idx_lr] != 'X' and int(arr[idx_tb-1][idx_lr]) < int(arr[idx_tb][idx_lr]) and \
                arr[idx_tb+1][idx_lr] != 'X' and int(arr[idx_tb+1][idx_lr]) < int(arr[idx_tb][idx_lr]) and \
                arr[idx_tb][idx_lr-1] != 'X' and int(arr[idx_tb][idx_lr-1]) < int(arr[idx_tb][idx_lr]) and \
                arr[idx_tb][idx_lr+1] != 'X' and int(arr[idx_tb][idx_lr+1]) < int(arr[idx_tb][idx_lr]):
                    arr[idx_tb][idx_lr] = 'X'

if __name__ == '__main__':
    n = input()
    
    arr = []
    
    for _ in xrange(n):
        line = list(raw_input())
        arr.append(line)
        
    transformCavity(arr, n)
    
    for line in arr:
        print ''.join(line)
```
