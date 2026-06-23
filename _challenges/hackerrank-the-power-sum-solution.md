---
title: "HackerRank 'The Power Sum' Solution"
date: "2018-09-22"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Find the number of ways that a given integer, X , can be expressed as the sum of the Nth powers of unique, natural numbers.

For example, if X = 13 and N = 2, we have to find all combinations of unique squares adding up to 13. The only solution is 2^2 + 3^2.

##### Link

[The Power Sum](https://www.hackerrank.com/challenges/the-power-sum)

##### Complexity:

time complexity is `O(N!)`

space complexity is `O(1)`

##### Execution:

This solution does not use DP or memoisation, but it does not time out. We know that it needs to explore all P! possible combinations where P is floor(sqrt(X, N)). Since maximum X is 1000, this should still compute in real time.

Keep in mind that all integers in the solution have to be unique. So for X = 3, the solution 1^2 + 1^2 + 1^2 is not acceptable.

Did I mention that I dislike both NP-Hard problems and recursion? No? So, now you know.

##### Solution:

```python

def powerSum(X, N, current = 1):
    pw = pow(current, N)
    if pw &gt; X:
        return 0
    elif pw == X:
        return 1
    else:
        return powerSum(X, N, current+1) + powerSum(X-pw, N, current+1)
```
