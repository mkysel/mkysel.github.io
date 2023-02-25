---
title: "HackerRank 'CamelCase' Solution"
date: "2018-09-13"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Alice wrote a sequence of words in [CamelCase](https://en.wikipedia.org/wiki/CamelCase) as a string of letters, , having the following properties:

- It is a concatenation of one or more _words_ consisting of English letters.
- All letters in the first word are _lowercase_.
- For each of the subsequent words, the first letter is _uppercase_ and rest of the letters are _lowercase_.

Given s , print the number of words in s on a new line.

For example, s = OneTwoThree . There are 3 words in the string.

##### Link

[CamelCase](https://www.hackerrank.com/challenges/camelcase)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Since the input always has at least 1 character, we can assume that there will always be at least one word. Each upper case later identifies the next word. So the result is number of capital letters + 1.

##### Solution:

```python

s = raw_input().strip()

cnt = 1

for c in s:
    if c.isupper():
        cnt += 1
        
print cnt
```
