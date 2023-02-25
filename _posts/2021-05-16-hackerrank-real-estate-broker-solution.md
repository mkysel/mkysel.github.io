---
title: "HackerRank 'Real Estate Broker' Solution"
date: "2021-05-16"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
excerpt: You are a real estate broker in ancient Knossos. You have m unsold houses, and each house j has an area, xj, and a minimum price, yj. You also have n clients, and each client i wants a house with an area greater than ai and a price less than or equal to bi. Each client can buy at most one house, and each house can have at most one owner. What is the maximum number of houses you can sell?
---

##### Short Problem Definition:

You are a real estate broker in ancient Knossos. You have m unsold houses, and each house j has an area, xj, and a minimum price, yj. You also have n clients, and each client i wants a house with an area greater than ai and a price less than or equal to bi.

Each client can buy _at most_ one house, and each house can have _at most_ one owner. What is the maximum number of houses you can sell?

##### Link

[Real Estate Broker](https://www.hackerrank.com/challenges/real-estate-broker/problem)

##### Complexity:

time complexity is `O(NlogN + MlogM + N\*M)`

space complexity is `O(1)`

##### Execution:

This challenge sounds pretty complex, but this greedy solution works perfectly. Sort houses by size, start selling the smallest houses first. Bigger houses can always be sold since there is a bigger pool of buyers for them. Sell each house to the buyer that has the smallest pool of money available.

Don't overcomplicate the problem with graph algorithms :)

##### Solution:

```python
#!/bin/python

from __future__ import print_function

import os
import sys

#
# Complete the realEstateBroker function below.
#
def realEstateBroker(clients, houses):
    houses = sorted(houses)
    clients = sorted(clients, key = lambda x: x[1])
    
    sold = 0
    
    for house in houses:
        for client in clients:
            if house[0] >= client[0] and house[1] <= client[1]:
                sold += 1
                clients.remove(client)
                break
    
    return sold

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    nm = raw_input().split()

    n = int(nm[0])

    m = int(nm[1])

    clients = []

    for _ in xrange(n):
        clients.append(map(int, raw_input().rstrip().split()))

    houses = []

    for _ in xrange(m):
        houses.append(map(int, raw_input().rstrip().split()))

    result = realEstateBroker(clients, houses)

    fptr.write(str(result) + '\n')

    fptr.close()
```
