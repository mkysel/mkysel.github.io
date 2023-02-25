---
title: "HackerRank 'Sherlock and Array' Solution"
date: "2015-03-31"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Watson gives Sherlock an array A of length N. Then he asks him to determine if there exists an element in the array such that the sum of the elements on its left is equal to the sum of the elements on its right. If there are no elements to the left/right, then the sum is considered to be zero. Formally, find an i, such that, A1+A2...Ai-1 =Ai+1+Ai+2...AN

##### Link

[Sherlock and Array](https://www.hackerrank.com/challenges/sherlock-and-array)

##### Complexity:

time complexity is O(n)

space complexity is O(1)

##### Execution:

I first solved the problem using prefix sums. Afterwards I realized that it can be done without additional space complexity.

##### Solution:

```python

#!/usr/bin/py
def arraySherlock(a):   
   
    left_idx = 0
    right_idx = len(a) - 1
    
    left_sum = a[left_idx]
    right_sum = a[right_idx]
    
    while left_idx != right_idx:
        if left_sum < right_sum:
            left_idx += 1
            left_sum += a[left_idx]
        else:
            right_idx -= 1
            right_sum += a[right_idx]
    
    return left_sum == right_sum
    
if __name__ == '__main__':
    t = int(raw_input())
    for _ in xrange(t):
        n = int(raw_input())
        a = map(int, raw_input().split())
        if arraySherlock(a):
            print "YES"
        else:
            print "NO"
```
