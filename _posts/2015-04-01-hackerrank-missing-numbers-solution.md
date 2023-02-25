---
title: "HackerRank 'Missing Numbers' Solution"
date: "2015-04-01"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Numeros, the Artist, had two lists A and B, such that B was a permutation of A. Numeros was very proud of these lists. Unfortunately, while transporting them from one exhibition to another, some numbers from A got left out. Can you find the numbers missing?

##### Link

[Sherlock and Array](https://www.hackerrank.com/challenges/sherlock-and-array)

##### Complexity:

time complexity is `O(n)`

space complexity is `O(n)`

##### Execution:

The problem statement informs us, that there are only 100 different values. This calls for a counting sort.0

##### Solution:

```python

#!/usr/bin/py
def solveMissing(n, m):
    n_cnt = [0] * 101
    m_cnt = [0] * 101
    offset = min(m)
    
    for ele in m:
        m_cnt[ele-offset] += 1
    
    for ele in n:
        n_cnt[ele-offset] += 1
    
    for idx in xrange(101):
        if m_cnt[idx] != n_cnt[idx]:
            print idx + offset,
    
    
if __name__ == '__main__':
    n = int(raw_input())
    arr = map(int, raw_input().split())
    m = int(raw_input())
    arr2 = map(int, raw_input().split())
    solveMissing(arr, arr2)
```
