---
title: "HackerRank 'Birthday Cake Candles' Solution"
date: "2019-05-08"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

You are in charge of the cake for your niece's birthday and have decided the cake will have one candle for each year of her total age. When she blows out the candles, she’ll only be able to blow out the tallest ones. Your task is to find out how many candles she can successfully blow out.

##### Link

[Birthday Cake Candles](https://www.hackerrank.com/challenges/birthday-cake-candles/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Keep track of the tallest one along with the count.

##### Solution:

```python
#!/bin/python

import sys

n = int(raw_input().strip())
height = map(int,raw_input().strip().split(' '))

cnt = 0
running_top = 0
for candle in height:
    if (candle > running_top):
        cnt = 1
        running_top = candle
    elif candle == running_top:
        cnt += 1
        
print cnt
```
