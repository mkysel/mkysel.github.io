---
title: "HackerRank 'Apple and Orange' Solution"
date: "2020-05-13"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Sam's house has an apple tree and an orange tree that yield an abundance of fruit. In the diagram below, the red region denotes his house, where _s_ is the start point, and _i_ is the endpoint. The apple tree is to the left of his house, and the orange tree is to its right. You can assume the trees are located on a single point, where the apple tree is at point _a_, and the orange tree is at point _b_.

##### Link

[Apple And Orange](https://www.hackerrank.com/challenges/apple-and-orange)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Follow the problem specification.

##### Solution:

```python
def solve(house_left, house_right, tree, elements):
    cnt = 0
    for ele in elements:
        if tree+ele >= house_left and tree+ele <= house_right:
           cnt += 1
    return cnt
```
