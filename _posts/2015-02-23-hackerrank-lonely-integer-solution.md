---
title: "HackerRank 'Lonely Integer' Solution"
date: "2015-02-23"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

There are _N_ integers in an array _A_. All but one integer occur in pairs. Your task is to find out the number that occurs only once.

##### Link

[Lonely Integer](https://www.hackerrank.com/challenges/lonely-integer)

##### Complexity:

time complexity is O(N);

space complexity is O(1)

##### Execution:

XORing two equal numbers cancels them out. XOR all numbers together.

##### Solution:

```python

#!/usr/bin/py
def lonelyinteger(a):
    answer = 0
    for candidate in a:
        answer ^= candidate
    return answer
if __name__ == '__main__':
    a = input()
    b = map(int, raw_input().strip().split(" "))
    print lonelyinteger(b)
```
