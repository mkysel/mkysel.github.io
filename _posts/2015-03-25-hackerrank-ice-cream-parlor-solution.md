---
title: "HackerRank 'Ice Cream Parlor' Solution"
date: "2015-03-25"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Sunny and Johnny together have _M_ dollars they want to spend on ice cream. The parlor offers _N_ flavors, and they want to choose two flavors so that they end up spending the whole amount.

You are given the cost of these flavors. The cost of the _i_th flavor is denoted by _ci_. You have to display the indices of the two flavors whose sum is _M_.

##### Link

[Ice Cream Parlor](https://www.hackerrank.com/challenges/icecream-parlor)

##### Complexity:

time complexity is O(n)

space complexity is O(1)

##### Execution:

I genuinely don't like n^2 solutions. But I could not find anything better.

UPDATE(18/05/16): I found an O(N) solution. As always the complexity of the hash-map can be disputed. I prefer tree-maps which would give us a guaranteed runtime complexity of O(N\*log(N)).

##### Solution:

```python
#!/usr/bin/py
def icecream(flavors, M):
    # O(N) complexity
    flavor_map = {}
    for idx, flavor in enumerate(flavors):
        residual = (M - flavor)
        if residual in flavor_map:
            return sorted([flavor_map[residual], idx])
        if not flavor in flavor_map:
            flavor_map[flavor] = idx

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        m = input()
        n = input()
        flavors = map(int, raw_input().split())
        result_arr = icecream(flavors, m)
        print result_arr[0]+1, result_arr[1]+1
```

* * *

```python
#!/usr/bin/py
def icecream(flavors, M):
    # O(N^2) complexity
    for idx in xrange(len(flavors)-1):
        for idx2 in xrange(idx+1, len(flavors)):
            if flavors[idx] + flavors[idx2] == M:
                return [idx+1, idx2]

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        m = input()
        n = input()
        flavors = map(int, raw_input().split())
        result_arr = icecream(flavors, m)
        print result_arr[0]+1, result_arr[1]+1
```
