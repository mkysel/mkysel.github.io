---
title: "HackerRank 'HackerRank in a String!' Solution"
date: "2018-09-16"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

We say that a string contains the word `hackerrank` if a [subsequence](https://en.wikipedia.org/wiki/Subsequence) of its characters spell the word `hackerrank`. For example, if string s = haacckkerrannkk it does contain `hackerrank`, but s = haacckkerannk does not. In the second case, the second `r` is missing. If we reorder the first string as , it no longer contains the subsequence due to ordering.

##### Link

[HackerRank in a String!](https://www.hackerrank.com/challenges/hackerrank-in-a-string)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Keep two pointers. One to the expected string (needle) and one to the input string. If you find the needle in the haystack before you run out of characters, you are good.

##### Solution:

```python

def hackerrankInString(s):
    needle = 'hackerrank'
    idx_in_needle = 0
    for c in s:
        if c == needle[idx_in_needle]:
            idx_in_needle += 1
        if idx_in_needle == len(needle):
            break
            
    if idx_in_needle == len(needle):
        return "YES"
    else: 
        return "NO"
```
