---
title: "HackerRank 'Divisible Sum Pairs' Solution"
date: "2016-07-03"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

You are given an array of n integers and a positive integer, k. Find and print the number of (i,j) pairs where i < j and ai + aj is evenly divisible by k.

##### Link

[Divisible Sum Pairs](https://www.hackerrank.com/challenges/divisible-sum-pairs)

##### Complexity:

time complexity is O(N^2)

space complexity is O(1)

##### Execution:

Brute force search.

##### Solution:

```python

n,k = raw_input().strip().split(' ')
n,k = [int(n),int(k)]
a = map(int,raw_input().strip().split(' '))
count=0
for i in xrange(len(a)):
    for j in xrange(i+1,len(a)):
        if (a[i]+a[j]) % k == 0:
            count+=1
        
print count
```
