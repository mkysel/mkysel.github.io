---
title: HackerRank 'Find Digits' Solution
date: '2015-03-10'
categories:
  - coding-challenge
  - hackerrank
  - python
---

##### Short Problem Definition:

You are given an integer N. Find the digits in this number that exactly divide N (division that leaves 0 as remainder) and display their count. For N=24, there are 2 digits (2 & 4). Both of these digits exactly divide 24. So our answer is 2.

##### Link

[Find Digits](https://www.hackerrank.com/challenges/find-digits)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Just follow the problem description. The problem can be optimized by creating a map of digits and their counts.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        a = input()
        count = 0
        for i in list(str(a)):
            if int(i) != 0 and a % int(i) == 0:
                count += 1
        print count
```
