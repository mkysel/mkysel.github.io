---
title: "HackerRank 'Mars Exploration' Solution"
date: "2018-09-15"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Sami's spaceship crashed on Mars! She sends a series of `SOS` messages to Earth for help.

##### Link

[Mars Exploration](https://www.hackerrank.com/challenges/mars-exploration)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

We know that the message is basically a lot of concatenated SOS strings. There is no magic to this one.

##### Solution:

```python

S = raw_input().strip()

errors = 0

for i in xrange(len(S)):
    if i % 3 == 0 and S[i] != 'S':
        errors += 1
    if i % 3 == 1 and S[i] != 'O':
        errors += 1
    if i % 3 == 2 and S[i] != 'S':
        errors += 1
        
        
print errors
```
