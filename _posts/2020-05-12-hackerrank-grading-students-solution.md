---
title: "HackerRank 'Grading Students' Solution"
date: "2020-05-12"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

HackerLand University has the following grading policy:

- Every student receives a _grade _in__ the inclusive range from _0_ to _100_.
- Any _grade_ less than _40_ is a failing grade.

##### Link

[Grading Students](https://www.hackerrank.com/challenges/grading/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

Follow the problem specification. The solution could be further optimized to remove all unnecessary copies and the whole _res_ array.

##### Solution:

```python
#!/bin/python

from __future__ import print_function

import os
import sys

#
# Complete the gradingStudents function below.
#
def gradingStudents(grades):
    res = []
    for grade in grades:
        if grade >= 38 and grade % 5 >= 3:
            grade = grade + 5 - (grade % 5)
        res.append(grade)
    return res

if __name__ == '__main__':
    f = open(os.environ['OUTPUT_PATH'], 'w')

    n = int(raw_input())

    grades = []

    for _ in xrange(n):
        grades_item = int(raw_input())
        grades.append(grades_item)

    result = gradingStudents(grades)

    f.write('\n'.join(map(str, result)))
    f.write('\n')

    f.close()
```
