---
title: "HackerRank 'Electronics Shop' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Monica wants to buy a keyboard and a USB drive from her favorite electronics store. The store has several models of each. Monica wants to spend as much as possible for the 2 items, given her budget.

Given the price lists for the store's keyboards and USB drives, and Monica's budget, find and print the amount of money Monica will spend. If she doesn't have enough money to both a keyboard _and_ a USB drive, print `-1` instead. She will buy only the two required items.

##### Link

[Electronics Shop](https://www.hackerrank.com/challenges/electronics-shop/problem)

##### Complexity:

time complexity is `O(N\*M)`

space complexity is `O(1)`

##### Execution:

The solution has a N\*M complexity. If the range for B was significantly smaller, one could consider alternatives.

##### Solution:

```python
def getMoneySpent(keyboards, drives, s):
    spend = -1

    for v1 in keyboards:
        for v2 in drives:
            if v1 + v2 <=s:
                spend = max(spend, v1 + v2)

    return spend
```
