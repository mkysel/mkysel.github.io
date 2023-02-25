---
title: "HackerRank 'Common Child' Solution"
date: "2015-08-30"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given two strings a and b of equal length, what's the longest string (S) that can be constructed such that it is a child of both?

A string x is said to be a child of a string y if x can be formed by deleting 0 or more characters from y.

##### Link

[Common Child](https://www.hackerrank.com/challenges/common-child)

##### Complexity:

time complexity is O(N\*M)

space complexity is O(N\*M)

##### Execution:

This is a longest common subsequence problem in disguise. I encourage you to look at a good explanation [here](http://www.geeksforgeeks.org/dynamic-programming-set-4-longest-common-subsequence/).

##### Solution:

```python
def lcs(a, b):
    lengths = [[0 for j in range(len(b)+1)] for i in range(len(a)+1)]
    for i, x in enumerate(a):
        for j, y in enumerate(b):
            if x == y:
                lengths[i+1][j+1] = lengths[i][j] + 1
            else:
                lengths[i+1][j+1] = \
                    max(lengths[i+1][j], lengths[i][j+1])
    
    return lengths[-1][-1]

def main():
    s1 = raw_input()
    s2 = raw_input()
    print lcs(s1,s2)
    
if __name__ == '__main__':
    main()
```
