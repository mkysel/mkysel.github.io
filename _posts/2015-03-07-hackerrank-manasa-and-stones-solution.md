---
title: "HackerRank 'Manasa and Stones' Solution"
date: "2015-03-07"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Manasa is out on a hike with friends. She finds a trail of stones with numbers on them. She starts following the trail and notices that two consecutive stones have a difference of either aor b. Legend has it that there is a treasure trove at the end of the trail and if Manasa can guess the value of the last stone, the treasure would be hers. Given that the number on the first stone was 0, find all the possible values for the number on the last stone.

##### Link

[Manasa and Stones](https://www.hackerrank.com/challenges/manasa-and-stones)

##### Complexity:

time complexity is O(N);

space complexity is O(N)

##### Execution:

There are two distinct values. It is basically a combination with repetition. The order of the stones does not matter.

##### Solution:

```python

#!/usr/bin/py

def manasa(n, a, b):
    solutions = set()
    for i in xrange(n):
        solutions.add(a * i + b * (n-i-1))

    return solutions

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        n = input()
        a = input()
        b = input()
        l = sorted(list(manasa(n, a, b)))
        print ' '.join(map(str, l))
```
