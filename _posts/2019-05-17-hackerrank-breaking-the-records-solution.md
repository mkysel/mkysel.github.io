---
title: "HackerRank 'Breaking The Records' Solution"
date: "2019-05-17"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Maria plays college basketball and wants to go pro. Each season she maintains a record of her play. She tabulates the number of times she breaks her season record for _most points_ and _least points_ in a game. Points scored in the first game establish her record for the season, and she begins counting from there.

##### Link

[Breaking The Records](https://www.hackerrank.com/challenges/breaking-best-and-worst-records/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Just track the min and max.

##### Solution:

```python
#!/bin/python

import sys

def getRecord(s):
    min_ele = s[0]
    max_ele = s[0]
    min_cnt = 0
    max_cnt = 0
    
    for ele in s:
        if ele > max_ele:
            max_ele = ele
            max_cnt += 1
        if ele < min_ele:
            min_ele = ele
            min_cnt += 1
    
    return [max_cnt, min_cnt]
    
n = int(raw_input().strip())
s = map(int, raw_input().strip().split(' '))
result = getRecord(s)
print " ".join(map(str, result))
```
