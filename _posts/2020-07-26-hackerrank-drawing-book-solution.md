---
title: "HackerRank 'Drawing Book' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Brie’s Drawing teacher asks her class to open their books to a page number. Brie can either start turning pages from the front of the book or from the back of the book. She always turns pages one at a time. 

##### Link

[Drawing Book](https://www.hackerrank.com/challenges/drawing-book/problem)

##### Complexity:

time complexity is O(1)

space complexity is O(1)

##### Execution:

Think of this as a 0-indexed book. Use integer division.

##### Solution:

```python
def solve(n, p):
    last_letter = n // 2
    goal_letter = p // 2
    return min(goal_letter, last_letter-goal_letter)
```
