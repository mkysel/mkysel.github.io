---
title: "Codility 'WireBurnouts' 2012 Psi Solution"
date: "2014-12-22"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

While removing edges from a mesh grid, find the moment when there ceases to be a connection between opposite corners.

##### Link

[WireBurnouts](https://codility.com/programmers/challenges/psi2012)

##### Complexity:

expected worst-case time complexity is O(N2\*log(N));

expected worst-case space complexity is O(N2)

##### Execution:

This problem screams for the use of a union-find. The hard part was to identify that union-find has no effective delete operation and you first need to add all remaining wires and start from the end. I created a wrapper class to effectively pack the input data into a set. Parsing the whole input for each union candidate (the _not in burnouts_ part) would result in O(N²xN²). By using a set (hash O(1) in Python, RB-tree O(log(N)) in C++)   you are down to N² x log(N). The union operation is also guaranteed to be log(N).

Beware though. (0,0) is the bottom left corner. First coordinate is X (right) and second coordinate is Y(up). I, for some reason, am used for the first coordinate to describe the row(down).

You can view my complete task timeline [here](https://codility.com/demo/results/demo5C37TA-3ZA/). How can someone solve this in 30 minutes? It took me longer to type the solution...

##### Solution:

```python

"""UnionFind.py

Union-find data structure. Based on Josiah Carlson's code,
http://aspn.activestate.com/ASPN/Cookbook/Python/Recipe/215912
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

class Wrapper:
    """ Wrapper class used purely for set hashing """
    def __init__(self, A, B, C):
        self.A = A
        self.B = B
        self.C = C

    def __eq__(self, other):
        return self.A == other.A and self.B == other.B and self.C == other.C

    def __hash__(self):
        return hash((self.A, self.B, self.C))

    def __str__(self):
        return "Wrapper: a is %s, b is %s, c is %s" % (self.A, self.B, self.C)

def solution(N, A, B, C):
    uf = UnionFind()
    grid_size = N*N

    burnouts = set()
    for idx in xrange(len(A)):
        burnouts.add(Wrapper(B[idx], A[idx], C[idx]))

    # the problem could be solved without an actual grid, using only an range index
    grid = range(grid_size + 2)

    # grid_size (N*N+1) represents a pseudo position before the first element
    # grid_size+1 (N*N+2) represents a pseudo position after the last element
    # when those two are connected, the current flows
    uf.union(grid[0], grid[grid_size])
    uf.union(grid[grid_size-1], grid[grid_size+1])

    # add all grids that did not burn out
    for left_right in xrange(N):
        for top_down in xrange(N):
            if top_down != N-1 and Wrapper(top_down, left_right, 0) not in burnouts:
                uf.union(grid[top_down*N+left_right], grid[(top_down+1)*N+left_right])
            if left_right != N-1 and Wrapper(top_down, left_right, 1) not in burnouts:
                uf.union(grid[top_down*N+left_right], grid[top_down*N+(left_right+1)])

    # as per problem specification
    if uf[grid[grid_size]] == uf[grid[grid_size+1]]:
        return -1

    # start adding burned out wires in a reversed manner
    for idx in reversed(xrange(len(C))):
        if C[idx] == 0:
            uf.union(grid[B[idx]*N+A[idx]], grid[(B[idx]+1)*N+A[idx]])
        else:
            uf.union(grid[B[idx]*N+A[idx]], grid[(B[idx])*N+A[idx]+1])

        if uf[grid[grid_size]] == uf[grid[grid_size+1]]:
            return idx+1

    # this should never happen
    return 0
```
