---
title: "HackerRank 'Anagram' Solution"
date: "2015-04-22"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Sid is obsessed with reading short stories. Being a CS student, he is doing some interesting frequency analysis with the books. He chooses strings S1 and S2 in such a way that |len(S1)−len(S2)|≤1

##### Link

[Anagram](https://www.hackerrank.com/challenges/anagram)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

Compare the frequency counts of the two parts.

##### Solution:

```python

#!/usr/bin/py

def buildMap(s):
    the_map = {}
    for char in s:
        if char not in the_map:
            the_map[char] = 1
        else:
            the_map[char] +=1
            
    return the_map       
    

def anagram(s):
    if len(s)%2 == 1:
        return -1
        
    mid = len(s)//2
    s1 = s[:mid]
    s2 = s[mid:]
    
    map1 = buildMap(s1)
    map2 = buildMap(s2)
    
    diff_cnt = 0
    for key in map2.keys():
        if key not in map1:
            diff_cnt += map2[key]
        else:
            diff_cnt += max(0, map2[key]-map1[key])
    
    return diff_cnt

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        s = raw_input()
        print anagram(s)
```
