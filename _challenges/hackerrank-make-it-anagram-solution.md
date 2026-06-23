---
title: "HackerRank 'Make it Anagram' Solution"
date: "2015-04-23"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Alice recently started learning about cryptography and found that anagrams are very useful. Two strings are anagrams of each other if they have same character set. For example strings`"bacdc"` and `"dcbac"` are anagrams, while strings `"bacdc"` and `"dcbad"` are not.

Alice decides on an encryption scheme involving 2 large strings where encryption is dependent on the minimum number of character deletions required to make the two strings anagrams. She need your help in finding out this number.

Given two strings (they can be of same or different length) help her in finding out the minimum number of character deletions required to make two strings anagrams. Any characters can be deleted from any of the strings.

##### Link

[Make it Anagram](https://www.hackerrank.com/challenges/make-it-anagram)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

Compare the frequency counts of the two parts.

The buildMap function can be replaced by:  `from collections import Counter`

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

def anagram(s1, s2):
    map1 = buildMap(s1)
    map2 = buildMap(s2)

    diff_cnt = 0
    for key in map2.keys():
        if key not in map1:
            diff_cnt += map2[key]
        else:
            diff_cnt += max(0, map2[key]-map1[key])

    for key in map1.keys():
        if key not in map2:
            diff_cnt += map1[key]
        else:
            diff_cnt += max(0, map1[key]-map2[key])

    return diff_cnt

if __name__ == '__main__':
    s1 = raw_input()
    s2 = raw_input()
    print anagram(s1, s2)
```
