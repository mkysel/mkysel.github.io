---
title: "HackerRank 'Sherlock and Watson' Solution"
date: "2015-03-20"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

John Watson performs an operation called Right Circular Rotation on an integer array \[a0, a1, ... an-1\]. Right Circular Rotation transforms the array from \[a0, a1, ... aN-1\] to \[aN-1, a0,... aN-2\].

He performs the operation K times and tests Sherlock's ability to identify the element at a particular position in the array. He asks Q queries. Each query consists of one integer, idx, for which you have to print the element at index idx in in the rotated array, i.e., aidx.

##### Link

[Sherlock and Watson](https://www.hackerrank.com/challenges/sherlock-and-watson)

##### Complexity:

time complexity is `O(1)`

space complexity is `O(1)`

##### Execution:

I could rotate the array before going into the test cases, but I can simply rotate the array on the fly by adding the rotation to the element index.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    n,k,q = map(int, raw_input().split())
    arr = map(int, raw_input().split())
    for _ in xrange(q):
        x = int(raw_input())
        print arr[(x-k)%n]
```
