---
title: "HackerRank 'Pangrams' Solution"
date: "2015-04-13"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Roy wanted to increase his typing speed for programming contests. So, his friend advised him to type the sentence "The quick brown fox jumps over the lazy dog" repeatedly, because it is a pangram. (Pangrams are sentences constructed by using every letter of the alphabet at least once.)

##### Link

[Pangrams](https://www.hackerrank.com/challenges/pangrams)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

There are 26 letters in the English alphabet. A sentence is a pangram, if it contains all 26 characters.

##### Solution:

```python

#!/usr/bin/py

def getCharCnt(s):
    return len(set(c.lower() for c in s if c != ' '))

if __name__ == '__main__':
    s = raw_input()
    if getCharCnt(s) == 26:
        print "pangram"
    else:
        print "not pangram"
```
