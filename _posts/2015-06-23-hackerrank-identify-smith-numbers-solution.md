---
title: "HackerRank 'Identify Smith Numbers' Solution"
date: "2015-06-23"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

A Smith number is a composite number, the sum of whose digits is the sum of the digits of its prime factors obtained as a result of prime factorization (excluding 1). The first few such numbers are 4, 22, 27, 58, 85, 94, and 121.

##### Link

[Identify Smith Numbers](https://www.hackerrank.com/challenges/identify-smith-numbers)

##### Complexity:

time complexity is O(sqrt(N))

space complexity is O(sqrt(N))

##### Execution:

There are many ways how to compute the prime composition of a number. I selected one with two optimization steps, there are many more. If there were many numbers to check I would use the Sieve of Eratosthenes to pre-compute the primes. The rest of the solution is straight forward. _Do not forget that prime factors can also contain multiple digits._

##### Solution:

```python

#!/usr/bin/py
def factors(n):
    f, fs = 3, []
    while n % 2 == 0:
        fs.append(2)
        n /= 2
    while f * f <= n:
        while n % f == 0:
            fs.append(f)
            n /= f
        f += 2
    if n > 1: fs.append(n)
    return fs

def getIntLetterCount(n):
    return sum([int(l) for l in list(str(n))])

def isSmithNumber(n):
    return sum([getIntLetterCount(f) for f in factors(n)]) == getIntLetterCount(n)

if __name__ == '__main__':
    n = input()

    if isSmithNumber(n):
        print 1
    else:
        print 0
```
