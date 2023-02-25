---
title: "HackerRank 'Sherlock and The Valid String' Solution"
date: "2018-09-19"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Sherlock considers a string to be _valid_ if all characters of the string appear the same number of times. It is also _valid_ if he can remove just 1 character at 1 index in the string, and the remaining characters will occur the same number of times. Given a string , determine if it is _valid_. If so, return `YES`, otherwise return `NO`.

##### Link

[Sherlock and The Valid String](https://www.hackerrank.com/challenges/sherlock-and-valid-string)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

This is one of the easier medium problems. Create a character occurrence map. Then create an occurrence-occurrence map. If all the occurrences are the same (size is 1) the string is valid. If there is exactly one character that occurs exactly once, it is also valid. Otherwise invalid

##### Solution:

```python

def isValid(S):
    char_map = Counter(S)
    char_occurence_map = Counter(char_map.values())

    if len(char_occurence_map) == 1:
        return True

    if len(char_occurence_map) == 2:
        for v in char_occurence_map.values():
            if v == 1:
                return True

    return False
```
