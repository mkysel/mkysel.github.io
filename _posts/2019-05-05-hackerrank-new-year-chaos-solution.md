---
title: "HackerRank 'New Year Chaos' Solution"
date: "2019-05-05"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

It's New Year's Day and everyone's in line for the Wonderland rollercoaster ride! There are a number of people queued up, and each person wears a sticker indicating their _initial_ position in the queue. Initial positions increment by _1_ from _1_ at the front of the line to N at the back.

Any person in the queue can bribe the person _directly in front_ of them to swap positions. If two people swap positions, they still wear the same sticker denoting their original places in line. One person can bribe _at most two others_. For example, if _n = 8_ and _Person 5_ bribes _Person 4_, the queue will look like this: 1, 2, 3, 5, 4, 6, 7, 8.

Fascinated by this chaotic queue, you decide you must know the minimum number of bribes that took place to get the queue into its current state!

##### Link

[New Year Chaos](https://www.hackerrank.com/challenges/new-year-chaos/)

##### Complexity:

time complexity is `O(N^2)`

space complexity is `O(1)`

##### Execution:

This task is solvable in O(N), but I first attempted to solve the Naive way. This same solution times if the inner loop is allowed to start at 0. Limiting the search to one position ahead of the original position passes all HR tests cases.

The moral of the story? Why write 100 lines of code, if 8 are enough.

##### Solution:

```python
def minimumBribes(q):
    moves = 0
    for pos, val in enumerate(q):
        if (val-1) - pos > 2:
            return "Too chaotic"
        for j in xrange(max(0,val-2), pos):
            if q[j] > val:
                moves+=1
    return moves
```
