---
title: "Codility 'ShortestAdjSeq' Solution"
date: "2020-08-21"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

A non-empty zero-indexed array A consisting of N integers is given. Two integers P and Q are called _adjacent in array A_ if there exists an index 0 ≤ K < N−1 such that:

- P = A\[K\] and Q = A\[K+1\], or
- Q = A\[K\] and P = A\[K+1\].

A non-empty zero-indexed sequence B consisting of M integers is called _adjacent in array A_ if the following conditions hold:

- B\[0\] = A\[0\];
- B\[M−1\] = A\[N−1\];
- B\[K\] and B\[K+1\] are adjacent in A for 0 ≤ K < M−1.

##### Link

[ShortestAdjSeq](https://app.codility.com/programmers/task/shortest_adj_seq/)

##### Complexity:

time complexity is `O(NlogN)`

space complexity is `O(N)`

##### Execution:

This is a graph problem, the shortest adjacent sequence is basically a shortest path in a bidirectional graph.

To calculate the shortest path I am using Breath-first-search or Dijkstra's algorithm.

I don't agree with the problem specification of one of the extreme cases such as \[1,2,1\]. I believe that the shortest path is 3, since the beginning and end are not adjacent. But it seems that Codility expects 1.

##### Solution:

```python
from collections import defaultdict

def bfs_shortest_path(paths, start, end):
    q = [(start, 1)]

    visited_paths = set()
    visited_paths.add(start)

    while q:
        key, distance = q.pop(0)
        new_distance = distance + 1
        for path in paths[key]:
            if path == end:
                return new_distance
            elif path not in visited_paths:
                visited_paths.add(path)
                q.append((path, new_distance))


def solution(A):
    if len(A) <= 2:
        return len(A)

    # I dont agree with this edge case based on the problem statement
    if A[0] == A[-1]:
        return 1

    paths = defaultdict(set)

    paths[A[0]].add(A[1])
    paths[A[-1]].add(A[-2])

    for i in range(1, len(A)-1):
        paths[A[i]].add(A[i-1])
        paths[A[i]].add(A[i+1])

    return bfs_shortest_path(paths, A[0], A[-1])
```
