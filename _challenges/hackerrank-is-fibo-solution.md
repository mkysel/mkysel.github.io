---
title: "HackerRank 'Is Fibo' Solution"
date: "2015-03-14"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given an integer, N. Write a program to determine if N is an element of the Fibonacci sequence.

##### Link

[Is Fibo](https://www.hackerrank.com/challenges/is-fibo)

##### Complexity:

time complexity is `O(15√(ϕn−(−ϕ)−n))`

space complexity is `O(15√(ϕn−(−ϕ)−n))`

##### Execution:

There are two methods:

A) generate all fibonacci numbers up to N and check if the candidates are in this set.

B) There is a mathematical function that can prove whether a number is in the Fibonacci sequence in sqrt(N) time. I am not going to explain this here. Read the [discussion](http://stackoverflow.com/questions/2432669/test-if-a-number-is-fibonacci) on SO if you are interested.

##### Solution:

```python

#!/usr/bin/py
def getFibo(N):
    v = [1,2]
    while v[-1] < N:
        v.append(v[-1]+v[-2])
    
    return set(v)

def isFibo(fiboSet, value):
    if value in fiboSet:
        return "IsFibo"
    return "IsNotFibo"

if __name__ == '__main__':
    t = input()
    values = []
    for _ in xrange(t):
        values.append(input())
    
    fiboSet = getFibo(max(values))
    
    for value in values:
        print isFibo(fiboSet, value)
```
