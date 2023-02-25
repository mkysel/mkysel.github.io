---
title: "HackerRank 'Sherlock and Pairs' Solution"
date: "2015-03-24"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Sherlock is given an array of N integers A0, A1 ... AN-1 by Watson. Now Watson asks Sherlock how many different pairs of indices i and j exist such that i is not equal to j but Ai is equal to Aj.

That is, Sherlock has to count total number of pairs of indices (i, j) where Ai = Aj AND i ≠ j.

##### Link

[Sherlock and Pairs](https://www.hackerrank.com/challenges/sherlock-and-pairs)

##### Complexity:

time complexity is O(n)

space complexity is O(n)

##### Execution:

The first step is to create a count of all integers. Next there are choose(k,2)\*2 distinct pairs for each integer count (this step similar to [Handshake](http://www.martinkysel.com/hackerrank-handshake-solution/ "HackerRank 'Handshake' Solution"), but you count i,j and j,i as two distinct pairs). Count those together and you arrive at the answer.

##### Solution:

```python

#!/usr/bin/py
def cnt_equals(arr):
    the_map = {}
    final_cnt = 0
    for value in arr:
        if value not in the_map:
            the_map[value] = 1
        else:
            the_map[value] += 1

    for value in the_map.values():
        if value != 1:
            final_cnt += (value*(value-1))

    return final_cnt

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        n = input()
        arr = map(int, raw_input().split())
        print cnt_equals(arr)
```
