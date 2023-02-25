---
title: "HackerRank 'Song of Pi' Solution"
date: "2015-06-24"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

That's the value of pi! (Ignoring the floating point) A song is a _pi song_ if the length of its words represent the value of pi.

##### Link

[Song of Pi](https://www.hackerrank.com/challenges/song-of-pi)

##### Complexity:

time complexity is O(N\*T)

space complexity is O(N)

##### Execution:

 

This problem is straight forward.

##### Solution:

```python

#!/usr/bin/py

PI = map(int, list("31415926535897932384626433833"))

def isPiSong(s):
    for idx, word in enumerate(s):
        if len(word) != PI[idx]:
            return False    
    return True
    
if __name__ == '__main__':
    t = input()
    for _ in range(t):
    	s = raw_input().split()
        if isPiSong(s):
            print "It's a pi song."
        else:
            print "It's not a pi song."
```
