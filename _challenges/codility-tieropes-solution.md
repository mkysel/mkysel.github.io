---
title: "Codility 'TieRopes' Solution"
date: "2014-08-25"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Tie adjacent ropes to achieve the maximum number of ropes of length >= K.

##### Link

[TieRopes](https://codility.com/demo/take-sample-test/tie_ropes)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(N)`

##### Execution:

I am a bit skeptical about the correctness of my solution. It gets 100/100 through...

##### Solution:

```python

def solution(K, A):
    cnt = 0
    current = 0
    for part in A:
        current += part
        if current >= K:
            cnt +=1
            current = 0

    return cnt
```
