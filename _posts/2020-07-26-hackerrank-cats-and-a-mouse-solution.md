---
title: "HackerRank 'Cats And A Mouse' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Two cats and a mouse are at various positions on a line. You will be given their starting positions. Your task is to determine which cat will reach the mouse first, assuming the mouse doesn't move and the cats travel at equal speed. If the cats arrive at the same time, the mouse will be allowed to move and it will escape while they fight.

##### Link

[Cats and a Mouse](https://www.hackerrank.com/challenges/cats-and-a-mouse/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

This one is trivial.

##### Solution:

```python
def catAndMouse(x, y, z):
    catA = abs(x-z)
    catB = abs(y-z)
    if (catA < catB):
        return "Cat A"
    elif (catB < catA):
        return "Cat B"
    else:
        return "Mouse C"
```
