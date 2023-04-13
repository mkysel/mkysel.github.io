---
title: "HackerRank 'String Similarity' Solution"
date: "2023-04-13"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
excerpt: For two strings A and B, we define the similarity of the strings to be the length of the longest prefix common to both strings.
---

##### Short Problem Definition:

For two strings A and B, we define the similarity of the strings to be the length of the longest prefix common to both strings. For example, the similarity of strings "abc" and "abd" is 2, while the similarity of strings "aaa" and "aaab" is 3.

Calculate the sum of similarities of a string S with each of it's suffixes.

##### Link

[String Similarity](https://www.hackerrank.com/challenges/string-similarity/)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(n)`

##### Execution:

The stringSimilarity function calculates the similarity of each suffix of the input string s with the original string using the Z algorithm.
This algorithm maintains two pointers 'l' and 'r' that define the substring with the longest similarity seen so far.
For each character 'i' in the string s, the algorithm calculates the similarity of the suffix starting at 'i' using the Z algorithm.
It then updates 'l' and 'r' if the similarity of the current suffix exceeds that of the current substring with the longest similarity.
Finally, the algorithm returns the sum of all similarities plus the length of the original string, since the similarity of the entire string is equal to its length.

##### Solution:

```python
#!/bin/python
#!/bin/python3

import math
import os
import random
import re
import sys

#
# Complete the 'stringSimilarity' function below.
#
# The function is expected to return an INTEGER.
# The function accepts STRING s as parameter.
#

def stringSimilarity(s):
    n = len(s)
    similarities = [0] * n
    l, r = 0, 0

    for i in range(1, n):
        if i <= r:
            similarities[i] = min(r-i+1, similarities[i-l])

        while i+similarities[i] < n and s[similarities[i]] == s[i+similarities[i]]:
            similarities[i] += 1

        if i+similarities[i]-1 > r:
            l = i
            r = i + similarities[i] - 1

    return sum(similarities) + n

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    t = int(input().strip())

    for t_itr in range(t):
        s = input()

        result = stringSimilarity(s)

        fptr.write(str(result) + '\n')

    fptr.close()

```
