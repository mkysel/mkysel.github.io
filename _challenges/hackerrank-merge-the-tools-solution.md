---
title: "HackerRank 'Merge The Tools' Solution"
date: "2020-07-30"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Split the string S into chunks T. Remove duplicates from T.

##### Link

[Merge The Tools](https://www.hackerrank.com/challenges/merge-the-tools/problem "https://www.hackerrank.com/challenges/merge-the-tools/problem")

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

First, split the string into chunks. In Python 2, use an ordered dictionary (that preserves insertion order) to discard duplicates. An ordered set would work too. The basic Sets/Maps are already ordered in Python 3.

##### Solution:

```python
from collections import OrderedDict

def merge_the_tools(string, k):
    chunks = [string[i:i+k] for i in range(0, len(string), k)]
    for chunk in chunks:
        print "".join(OrderedDict.fromkeys(chunk))
```
