---
title: "HackerRank 'Funny String' Solution"
date: "2015-04-10"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Suppose you have a string S which has length N and is indexed from 0 to N−1. String R is the reverse of the string S. The string S is funny if the condition |Si−Si−1|=|Ri−Ri−1| is true for every i from 1 to N−1.

##### Link

[Funny String](https://www.hackerrank.com/challenges/funny-string)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Just follow the instructions.

##### Solution:

```python

#!/usr/bin/py
def isStrFunny(s):
    s_len = len(s)
    idx = 0
    while idx < s_len//2:
        left_diff = abs(ord(s[idx]) - ord(s[idx+1]))
        right_diff = abs(ord(s[s_len-idx-1]) - ord(s[s_len-idx-2]))
        if left_diff != right_diff:
            return False
        idx += 1
    
    return True
    

if __name__ == '__main__':
    t = input()
    for _ in range(t):
    	s = raw_input()
        if isStrFunny(s):
            print "Funny"
        else:
            print "Not Funny"

```
