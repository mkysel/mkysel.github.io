---
title: "Codility 'StoneWall' Solution"
date: "2014-12-31"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Cover "Manhattan skyline" using the minimum number of rectangles.

##### Link

[StoneWall](https://codility.com/demo/take-sample-test/stone_wall)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(N)`

##### Execution:

The explanation to this challenge has been posted on the Codility web, you can read it [here](https://codility.com/media/train/solution-stone-wall.pdf).

##### Solution:

```python

def solution(H):
    block_cnt = 0

    stack = []

    for height in H:
        # remove all blocks that are bigger than my height
        while len(stack) != 0 and stack[-1] &amp;gt; height:
            stack.pop()

        if len(stack) != 0 and stack[-1] == height:
            # we already paid for this size
            pass
        else:
            # new block is required, push it's size to the stack
            block_cnt += 1
            stack.append(height)

    return block_cnt
```
