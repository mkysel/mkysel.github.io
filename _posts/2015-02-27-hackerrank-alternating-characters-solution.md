---
title: "HackerRank 'Alternating Characters' Solution"
date: "2015-02-27"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Shashank likes strings in which consecutive characters are different. For example, he likes ABABA, while he doesn't like ABAA. Given a string containing characters A and B only, he wants to change it into a string he likes. To do this, he is allowed to delete the characters in the string.

Your task is to find the minimum number of required deletions.

##### Link

[AlternatingChars](https://www.hackerrank.com/challenges/alternating-characters)

##### Complexity:

time complexity is O(N\*T);

space complexity is O(1)

##### Execution:

If two following characters are the same, you have to delete one of them. A sequence of x equal characters will require (x-1) deletions.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    for _ in range(t):
    	s = raw_input()
    	delete_cnt = 0
    	for i in range(1,len(s)):
            if s[i] == s[i-1]:
            	delete_cnt +=1
        print delete_cnt
```
