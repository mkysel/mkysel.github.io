---
title: "HackerRank 'Friend Circle Queries' Solution"
date: "2021-01-24"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
excerpt: The population of HackerWorld is 10^9. Initially, none of the people are friends with each other. In order to start a friendship, two persons a and b have to shake hands, where `1 <= a,b <= 10^9`. The friendship relation is transitive, that is if a and b shake hands with each other, a and friends of a become friends with b and friends of b.
---

##### Short Problem Definition:

The population of HackerWorld is 10^9. Initially, none of the people are friends with each other. In order to start a friendship, two persons a and b have to shake hands, where `1 <= a,b <= 10^9`. The friendship relation is transitive, that is if a and b shake hands with each other, a and friends of a become friends with b and friends of b.

##### Link

[Friend Circle Queries](https://www.hackerrank.com/challenges/friend-circle-queries/problem)

##### Complexity:

time complexity is `O(n(logq+logn))`

space complexity is `O(N)`

##### Execution:

This is a typical Union-find problem statement. The particular UF implementation I use does not track groups and their sizes. The best way to determine the current largest group is to assume that the group that was just merged is the largest one.

##### Solution:

```python
#!/bin/python

import math
import os
import random
import re
import sys
from collections import defaultdict
 
"""UnionFind.py
 
Union-find data structure. Based on Josiah Carlson's code,
<a class="vglnk" href="http://aspn.activestate.com/ASPN/Cookbook/Python/Recipe/215912" rel="nofollow"><span>http</span><span>://</span><span>aspn</span><span>.</span><span>activestate</span><span>.</span><span>com</span><span>/</span><span>ASPN</span><span>/</span><span>Cookbook</span><span>/</span><span>Python</span><span>/</span><span>Recipe</span><span>/</span><span>215912</span></a>
with significant additional changes by D. Eppstein.
"""
 
class UnionFind:
    """Union-find data structure.
 
    Each unionFind instance X maintains a family of disjoint sets of
    hashable objects, supporting the following two methods:
 
    - X[item] returns a name for the set containing the given item.
      Each set is named by an arbitrarily-chosen one of its members; as
      long as the set remains unchanged it will keep the same name. If
      the item is not yet part of a set in X, a new singleton set is
      created for it.
 
    - X.union(item1, item2, ...) merges the sets containing each item
      into a single larger set.  If any item is not yet part of a set
      in X, it is added to X as one of the members of the merged set.
    """
 
    def __init__(self):
        """Create a new empty union-find structure."""
        self.weights = {}
        self.parents = {}
 
    def __getitem__(self, object):
        """Find and return the name of the set containing the object."""
 
        # check for previously unknown object
        if object not in self.parents:
            self.parents[object] = object
            self.weights[object] = 1
            return object
 
        # find path of objects leading to the root
        path = [object]
        root = self.parents[object]
        while root != path[-1]:
            path.append(root)
            root = self.parents[root]
 
        # compress the path and return
        for ancestor in path:
            self.parents[ancestor] = root
        return root
 
    def __iter__(self):
        """Iterate through all items ever found or unioned by this structure."""
        return iter(self.parents)
 
    def union(self, *objects):
        """Find the sets containing the objects and merge them all."""
        roots = [self[x] for x in objects]
        heaviest = max([(self.weights[r],r) for r in roots])[1]
        for r in roots:
            if r != heaviest:
                self.weights[heaviest] += self.weights[r]
                self.parents[r] = heaviest

# Complete the maxCircle function below.
def maxCircle(queries):
    uf = UnionFind()
    
    largest_element = 0
    result = []
    
    for query in queries:
        uf.union(query[0], query[1])
        current_grouping = uf.weights[uf[query[0]]]
        largest_element = max(largest_element, current_grouping)
        result.append(largest_element)
        
    return result

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    q = int(raw_input())

    queries = []

    for _ in xrange(q):
        queries.append(map(int, raw_input().rstrip().split()))

    ans = maxCircle(queries)

    fptr.write('\n'.join(map(str, ans)))
    fptr.write('\n')

    fptr.close()
```
