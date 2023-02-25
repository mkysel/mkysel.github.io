---
title: "HackerRank 'Library Fine' Solution"
date: "2015-06-19"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

The Head Librarian at a library wants you to make a program that calculates the fine for returning the book after the return date. You are given the actual and the expected return dates

##### Link

[Library Fine](https://www.hackerrank.com/challenges/library-fine)

##### Complexity:

time complexity is O(?)

space complexity is O(?)

##### Execution:

HackerRank does not support the dateutil library. This should have been computed in terms of date differences and not using if/else branches. Here I use the date library for convenience and readability.

##### Solution:

```python

#!/usr/bin/py
from datetime import date

def calculateHackos(actual, expected):
    actual_date = date(actual[2], actual[1], actual[0])
    expected_date = date(expected[2], expected[1], expected[0])
    
    if actual_date.year > expected_date.year: return 10000
    if actual_date.year < expected_date.year: return 0 
    if actual_date.month > expected_date.month: return (actual_date.month - expected_date.month) * 500
    if actual_date.month < expected_date.month: return 0
    if actual_date.day > expected_date.day: return (actual_date.day - expected_date.day) * 15
    return 0    

if __name__ == '__main__':
    actual = map(int, raw_input().split())
    expected = map(int, raw_input().split())
    print calculateHackos(actual, expected)
    
```
