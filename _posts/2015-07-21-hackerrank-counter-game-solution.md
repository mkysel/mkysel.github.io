---
title: "HackerRank 'Counter Game' Solution"
date: "2015-07-21"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Louise and Richard play a game. They have a counter set to N. Louise gets the first turn and the turns alternate thereafter. In the game, they perform the following operations.

- If N is not a power of 2, reduce the counter by the largest power of 2 less than N.
- If N is a power of 2, reduce the counter by half of N.
- The resultant value is the new N which is again used for subsequent operations.

The game ends when the counter reduces to 1, i.e., N == 1, and the last person to make a valid move wins.

Given N, your task is to find the winner of the game.

##### Link

[Counter Game](https://www.hackerrank.com/challenges/counter-game)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

I really enjoyed this one. It can be solved without any while loops as long as you are aware of the input size. You get the previous power of two by computing the next one and shifting right. I consider it funny that this simple solution is not mentioned in the editorial.

##### Solution:

```python

#!/usr/bin/py

def getClosestSmaller(x):
    x |= x >> 1
    x |= x >> 2
    x |= x >> 4
    x |= x >> 8
    x |= x >> 16
    x |= x >> 32
    x = x + 1
    x = x >> 1
    return x

def getNrReductions(x):
    reductions = 1

    while (x != 1):
        #print x
        if x & (x - 1):
            #print "x is not power of two"
            x -= getClosestSmaller(x)
        else:
            #print "x is power of two"
            x /= 2
        reductions += 1    

    return reductions

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        n = input()
        if getNrReductions(n) % 2 != 0:
            print "Richard"
        else:
            print "Louise"
```
