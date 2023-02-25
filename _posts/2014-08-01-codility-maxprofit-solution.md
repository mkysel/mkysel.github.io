---
title: "Codility 'MaxProfit' Solution"
date: "2014-08-01"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Given a log of stock prices compute the maximum possible earning.

##### Link

[MaxProfit](https://codility.com/demo/take-sample-test/max_profit)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(1)

##### Execution:

Keep the minimal value up to day. The profit on day i is _profit\[i\] - min\_profit_.

##### Solution:

```python

def solution(A):
    max_profit = 0
    max_day = 0
    min_day = 200000
    
    for day in A:
        min_day = min(min_day, day)
        max_profit = max(max_profit, day-min_day)
    
    return max_profit
```
