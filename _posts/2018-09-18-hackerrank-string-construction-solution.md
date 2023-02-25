---
title: "HackerRank 'String Construction' Solution"
date: "2018-09-18"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Amanda has a string of lowercase letters that she wants to copy to a new string. She can perform the following operations with the given costs. She can perform them any number of times to construct a new string p:

- Append a character to the end of string p at a cost of 1 dollar.
- Choose any [substring](https://en.wikipedia.org/wiki/Substring) of p and append it to the end of  at no charge.

##### Link

[String Construction](https://www.hackerrank.com/challenges/string-construction)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

The solution sounds too easy, but it is still very simple. A substring of length 1 is still a substring. Each character in the final string needs to be copied once for 1$. Each other occurrence of that string can be copied for 0$. Aka just count the number of distinct letters in the expected string.

##### Solution:

```python

def stringConstruction(s):
    return len(set(s))
```
