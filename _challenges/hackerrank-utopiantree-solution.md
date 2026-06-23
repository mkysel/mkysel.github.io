---
title: "HackerRank 'Utopian Tree' Solution"
date: "2015-02-25"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

The Utopian Tree goes through 2 cycles of growth every year. The first growth cycle occurs during the spring, when it doubles in height. The second growth cycle occurs during the summer, when its height increases by 1 meter.  Now, a new Utopian Tree sapling is planted at the onset of spring. Its height is 1 meter. Can you find the height of the tree after N growth cycles?

##### Link

[Utopian Tree](https://www.hackerrank.com/challenges/utopian-tree)

##### Complexity:

time complexity is `O(N + max(N))`

space complexity is `O(max(N))`

##### Execution:

Just follow the problem description. I compute the height for all test cases at the same time. Printing them at the end.

##### Solution:

```python

#!/usr/bin/py
def printUtopianTree(cycles):
    heights = [1]

    for cycle_nr in xrange(1,max(cycles)+1):
    	if cycle_nr % 2 == 1:
            heights.append(heights[-1]*2)
    	else:
            heights.append(heights[-1]+1)

    for cycle in cycles:
    	print heights[cycle]

if __name__ == '__main__':
    n = int(raw_input())

    cycles = []

    for i in range(0,n):
    	cycles.append(int(raw_input()))

    printUtopianTree(cycles)
```
