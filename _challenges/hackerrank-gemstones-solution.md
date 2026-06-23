---
title: "HackerRank 'Gemstones' Solution"
date: "2015-04-15"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

John has discovered various rocks. Each rock is composed of various elements, and each element is represented by a lower-case Latin letter from 'a' to 'z'. An element can be present multiple times in a rock. An element is called a gem-element if it occurs at least once in each of the rocks.

Given the list of N rocks with their compositions, display the number of gem-elements that exist in those rocks.

##### Link

[Gemstones](https://www.hackerrank.com/challenges/gem-stones)

##### Complexity:

time complexity is `O(N\*T)`

space complexity is `O(N)`

##### Execution:

We count the number of elements that occur in all characters  sets. Just use set intersection.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    all_elements = set(raw_input())
    
    for _ in xrange(t-1):
        all_elements &= set(raw_input())
        
    print len(all_elements)
```
