---
title: "HackerRank 'Lisas Workbook' Solution"
date: "2016-06-22"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Lisa just got a new math workbook. A workbook contains exercise problems, grouped into chapters.

- .....

Lisa believes a problem to be _special_ if its index (within a chapter) is the same as the page number where it's located. Given the details for Lisa's workbook, can you count its number of _special_ problems?

##### Link

[Lisa's Workbook](https://www.hackerrank.com/challenges/bear-and-workbook)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

This brute force solution iterates over all pages in the final book keeping track of the page(offset) and chapter it is on. There can only be one special problem per page and therefore I check if there is _any_ problem that would match the criteria.

##### Solution:

```python

def findPages(N, K, P):
    cnt = 0
    offset = 1
    for chapter in P:
        pages = (chapter + K -1)/K
        for idx in xrange(pages):
            if offset >= (idx * K)+1 and offset <= min((idx+1)*K, chapter):
                cnt += 1
            offset += 1

    return cnt

if __name__ == '__main__':
    N, K = map(int, raw_input().split())
    P = map(int, raw_input().split())
    print findPages(N, K, P)
```
