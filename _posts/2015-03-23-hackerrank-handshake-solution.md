---
title: "HackerRank 'Handshake' Solution"
date: "2015-03-23"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

At the annual meeting of Board of Directors of Acme Inc, every one starts shaking hands with everyone else in the room. Given the fact that any two persons shake hand exactly once, Can you tell the total count of handshakes?

##### Link

[Handshake](https://www.hackerrank.com/challenges/handshake)

##### Complexity:

time complexity is O(n)

space complexity is O(n)

##### Execution:

I love this problem as it is a great reference for other pair solutions. There are choose(n,2) pairs in k elements. This reduces to n\*(n-1)/2. The if/else branch is not required, but I like to keep it in to be more verbose.

##### Solution:

```python

#!/usr/bin/py
def main():
    n = input()
    for _ in xrange(n):
        t = input()
        if (t == 1):
            print 0
        else:
            print t*(t-1)/2
            
    
if __name__ == '__main__':
    main()
```
