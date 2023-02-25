---
title: "HackerRank 'Balanced Strings' Solution"
date: "2021-05-16"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Consider a string, s, consisting only of the letters `a` and `b`. We say that string s is balanced if both of the following conditions are satisfied:

1. s has the same number of occurrences of `a` and `b`.
2. In each prefix of s, the number of occurrences of `a` and `b` differ by _at most_ 1.

Your task is to write a regular expression accepting only balanced strings.

##### Link

[Balanced Strings](https://www.hackerrank.com/contests/regular-expresso/challenges/balanced-strings/problem)

##### Execution:

Substrings can differ by at most 1 which means that the string must consist of either "ab" or "ba". Write a regex as such.

##### Solution:

```python
Regex_Pattern = r"^((ab)|(ba))*$"	# Do not delete 'r'.
```
