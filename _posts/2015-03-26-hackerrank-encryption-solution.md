---
title: "HackerRank 'Encryption' Solution"
date: "2015-03-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

An English text needs to be encrypted using the following encryption scheme. First, the spaces are removed from the text. Let L be the length of this text.

##### Link

[Encryption](https://www.hackerrank.com/challenges/encryption)

##### Complexity:

time complexity is O(n)

space complexity is O(1)

##### Execution:

You do not need to create all the arrays. Just work with an offset and array slices.

##### Solution:

```python

#!/usr/bin/py
from math import sqrt, floor, ceil

if __name__ == '__main__':
    s = raw_input().replace(" ", "")
    columns = int(ceil(sqrt(len(s))))
    for c in xrange(columns):
        print s[[c::columns]],

```
