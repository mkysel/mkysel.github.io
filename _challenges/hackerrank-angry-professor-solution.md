---
title: "HackerRank 'AngryProfessor' Solution"
date: "2015-03-11"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

The professor is conducting a course on Discrete Mathematics to a class of N students. He is angry at the lack of their discipline, and he decides to cancel the class if there are less than K students present after the class starts.

Given the arrival time of each student, your task is to find out if the class gets cancelled or not.

##### Link

[Angry Professor](https://www.hackerrank.com/challenges/angry-professor)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Just count all students with arrival time <= 0.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        n, m = map(int, raw_input().split())
        A = map(int, raw_input().split())
        for x in A:
            if x <= 0:
                m -= 1
        if m <= 0:
            print "NO"
        else:
            print "YES"
```
