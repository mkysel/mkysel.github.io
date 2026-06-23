---
title: "HackerRank 'Sherlock and Valid String' Solution"
date: "2016-02-23"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Sherlock considers a string to be _valid_ if all characters of the string appear the same number of times. It is also _valid_ if he can remove just 1 character at 1 index in the string, and the remaining characters will occur the same number of times. Given a string , determine if it is _valid_. If so, return `YES`, otherwise return `NO`.

##### Link

[Sherlock and Valid String](https://www.hackerrank.com/challenges/sherlock-and-valid-string)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

The logic of the solution is as follows: count the character counts for each character.

- if they are all equal - it means that all characters occur exactly N times and there is no removal needed
- if 2 or more have less or more characters - there is no way to fix the string in just 1 removal
- if exactly 1 char has a different count than all other characters - remove this char completely and S is fixed.

EDIT 2020: HR added some extra test cases and the solution no longer worked. I updated it to pass all the cases including a secret "aabbbccc" case that HR is not testing for.

##### Solution:

```python
def isValid(S):
    char_map = Counter(S)
    char_occurence_map = Counter(char_map.values())

    if len(char_occurence_map) == 1:
        return True
 
    if len(char_occurence_map) == 2:
        k1, k2 = char_occurence_map.keys()
        v1, v2 = char_occurence_map.values()

        # there is exactly 1 extra symbol and it can be deleted
        if (k1 == 1 and v1 == 1) or (k2 == 1 and v2 == 1):
            return True

        # the is exactly 1 symbol that occurs an extra 1 time
        if (k1 == k2+1 and v1 == 1) or (k2 == k1+1 and v2 == 1):
            return True
 
    return False
```
