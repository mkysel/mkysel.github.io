---
title: "HackerRank 'Sock Merchant' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

John works at a clothing store. He has a large pile of socks that he must pair by color for sale. Given an array of integers representing the color of each sock, determine how many pairs of socks with matching colors there are.

##### Link

[Sock Merchant](https://www.hackerrank.com/challenges/sock-merchant/problem)

[Sales By Match](https://www.hackerrank.com/challenges/sock-merchant/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

Count the occurrence of every element. Use integer division to eliminate unpaired socks.

##### Solution:

```python
from collections import defaultdict

def sockMerchant(n, ar):
    d = defaultdict(int)

    for k in ar:
        d[k] += 1
        
    cnt = 0
    for ele in d.values():
        cnt += ele // 2
        
    return cnt
```
