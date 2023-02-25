---
title: "HackerRank 'Taum and B'day' Solution"
date: "2015-03-15"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Taum is planning to celebrate the birthday of his friend Diksha. There are two types of gifts that Diksha wants from Taum: one is black and the other is white. To make her happy, Taum has to buy B number of black gifts and W number of white gifts.

##### Link

[Taum and B'day](https://www.hackerrank.com/challenges/taum-and-bday)

##### Complexity:

time complexity is O(1)

space complexity is O(1)

##### Execution:

The cost for each present is either the cost of its category, or the cost of the other category + the conversion cost. Just select the minimum.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        b, w = map(int, raw_input().split())
        x, y, z = map(int, raw_input().split())
        
        print min(b*x, b*(y+z)) + min(w*(x+z), w*y)
```
