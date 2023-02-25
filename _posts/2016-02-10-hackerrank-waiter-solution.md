---
title: "HackerRank 'Waiter' Solution"
date: "2016-02-10"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are a waiter at a party. There are NN stacked plates. Each plate has a number written on it. You start picking up the plates from the top one by one and check whether the number written on the plate is divisible by a prime....

##### Link

[Waiter](https://www.hackerrank.com/challenges/waiter)

##### Complexity:

time complexity is `O(N\*Q)`

space complexity is `O(N)`

##### Execution:

First of all, generate primes using Sieve, or copy-paste a list of pre-generated primes. After each iteration the order of the remaining plates is reversed (you put the last on top and once finished you start from the top). That means that the solution has to have a stack, not a queue.

It is not possible to solve this in one pass. I would love to find such a solution.

##### Solution:

```python

generated_primes = [...]

def solveWaiter(N, Q, numbers):
    stack = []
    
    for value in numbers:
        if value % generated_primes[0] != 0:
            stack.append(value)
        else:
            print value
    
    
    for prime_idx in xrange(1, Q):
        leftover = []
        
        while stack:
            value = stack.pop()
            if value % generated_primes[prime_idx] != 0:
                leftover.append(value)
            else:
                print value
        stack = leftover
    
    for value in stack:
        print value

def main():
    N, Q = map(int, raw_input().split())
    
    numbers = map(int, raw_input().split())
    
    solveWaiter(N, Q, numbers)
    

if __name__ == '__main__':
    main()

```
