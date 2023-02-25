---
title: "HackerRank 'No Idea!' Solution"
date: "2020-07-30"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

There is an array of  integers. There are also **2** **disjoint sets**, A and ,B each containing m integers. You like all the integers in set A and dislike all the integers in set B. Your initial happiness is 0. For each a integer in the array, if A, you add 1 to your happiness. If b in B, you add -1 to your happiness. Otherwise, your happiness does not change. Output your final happiness at the end.

##### Link

[No Idea!](https://www.hackerrank.com/challenges/no-idea/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Make sure that you convert A and B into sets to reduce the lookup times.

##### Solution:

```python
def noIdea(N, A, B):
    happiness = 0

    A = set(A)
    B = set(B)

    for n in N:
        if n in A:
            happiness += 1
        elif n in B:
            happiness -= 1
    
    return happiness
```
