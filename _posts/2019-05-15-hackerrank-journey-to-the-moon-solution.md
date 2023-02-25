---
title: "HackerRank 'Journey To The Moon' Solution"
date: "2019-05-15"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

The member states of the UN are planning to send _2_ people to the moon. They want them to be from different countries. You will be given a list of pairs of astronaut ID's. Each pair is made of astronauts from the same country. Determine how many pairs of astronauts from different countries they can choose from.

##### Link

[Journey To The Moon](https://www.hackerrank.com/challenges/journey-to-the-moon/problem)

##### Complexity:

time complexity is O(a(N))

space complexity is O(N)

##### Execution:

The algorithm has two parts. Part one calculates the size of all countries with the use of Union-Find. Both operations on the data structure take O log-star time. The second part derives the number of all pairs based on country sizes. That algorithm is very similar to [Handshakes.](https://www.martinkysel.com/hackerrank-handshake-solution/)

I used the UF implementation from [ActiveState](http://code.activestate.com/recipes/215912-union-find-data-structure/). For more cool uses of Union-Find, see [WireBurnouts](https://www.martinkysel.com/codility-wireburnouts-2012-psi-solution/).

##### Solution:

```python
#!/bin/python

import math
import os
import random
import re
import sys
from collections import defaultdict

class UnionFind:
'''http://code.activestate.com/recipes/215912-union-find-data-structure/'''
    def __init__(self):
        self.num_weights = {}
        self.parent_pointers = {}
        self.num_to_objects = {}
        self.objects_to_num = {}
        self.__repr__ = self.__str__

    def insert_objects(self, objects):
        for object in objects:
            self.find(object);

    def find(self, object):
        if not object in self.objects_to_num:
            obj_num = len(self.objects_to_num)
            self.num_weights[obj_num] = 1
            self.objects_to_num[object] = obj_num
            self.num_to_objects[obj_num] = object
            self.parent_pointers[obj_num] = obj_num
            return object
        stk = [self.objects_to_num[object]]
        par = self.parent_pointers[stk[-1]]
        while par != stk[-1]:
            stk.append(par)
            par = self.parent_pointers[par]
        for i in stk:
            self.parent_pointers[i] = par
        return self.num_to_objects[par]

    def union(self, object1, object2):
        o1p = self.find(object1)
        o2p = self.find(object2)
        if o1p != o2p:
            on1 = self.objects_to_num[o1p]
            on2 = self.objects_to_num[o2p]
            w1 = self.num_weights[on1]
            w2 = self.num_weights[on2]
            if w1 < w2:
                o1p, o2p, on1, on2, w1, w2 = o2p, o1p, on2, on1, w2, w1
            self.num_weights[on1] = w1+w2
            del self.num_weights[on2]
            self.parent_pointers[on2] = on1

# Complete the journeyToMoon function below.
def journeyToMoon(n, astronauts):
    # generate disjoint sets
    uf = UnionFind()
    uf.insert_objects(xrange(n))

    for astronaut in astronauts:
        uf.union(astronaut[0], astronaut[1])

    # calculate sizes of countries
    country_sizes = defaultdict(int)
    for i in xrange(n):
        country_sizes[uf.find(i)] += 1

    # calculate number of viable pairs
    sm = 0
    result = 0
    for size in country_sizes.values():
        result += sm*size;
        sm += size; 

    return result

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    np = raw_input().split()

    n = int(np[0])

    p = int(np[1])

    astronaut = []

    for _ in xrange(p):
        astronaut.append(map(int, raw_input().rstrip().split()))

    result = journeyToMoon(n, astronaut)

    fptr.write(str(result) + '\n')

    fptr.close()
```
