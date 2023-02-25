---
title: "HackerRank 'Birthday Chocolate' Solution"
date: "2020-05-21"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

The member states of the UN are planning to send _2_ people to the moon. They want them to be from different countries. You will be given a list of pairs of astronaut ID's. Each pair is made of astronauts from the same country. Determine how many pairs of astronauts from different countries they can choose from.

##### Link

[Birthday Chocolate](https://www.hackerrank.com/challenges/the-birthday-bar/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Keep track of _M_elements at a time. Basically a simplified prefix sum.

##### Solution:

```python
#!/bin/python

import sys

def getWays(squares, d, m):
    cnt = 0
    q = squares[:m-1]
    for ele in squares[m-1:]:
        q.append(ele)
        if (sum(q) == d):
            cnt += 1
        q.pop(0)
    return cnt

n = int(raw_input().strip())
s = map(int, raw_input().strip().split(' '))
d,m = raw_input().strip().split(' ')
d,m = [int(d),int(m)]
result = getWays(s, d, m)
print(result)
```
