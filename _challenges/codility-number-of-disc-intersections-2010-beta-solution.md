---
title: "Codility 'NumberOfDiscIntersections' 2010 Beta Solution"
date: "2015-04-27"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute intersections between sequence of discs.

Given an array A of N integers, we draw N discs in a 2D plane such that the I-th disc is centered on (0,I) and has a radius of A\[I\]. We say that the J-th disc and K-th disc intersect if J ≠ K and J-th and K-th discs have at least one common point.

##### Link

[Number of Disc Intersections](https://codility.com/c/intro/demoX8DSMB-HAT)

##### Complexity:

expected worst-case time complexity is `O(N\*log(N))`

expected worst-case space complexity is `O(N)`

##### Execution:

In my mind this problem is very similar to the [Codility Fish](http://www.martinkysel.com/codility-fish-solution/ "Codility ‘Fish’ Solution"). You keep count of how many not-yet-ended disks there are. Every beginning has to cross all not-yet-ended disks. The best explanation on the web was written by [Luca](http://www.lucainvernizzi.net/blog/2014/11/21/codility-beta-challenge-number-of-disc-intersections/). His solution is much better than my C-style double-checked while loop. I therefore created a mixture of both :)

- Keep in mind that any pair of circles can intersect only once.
- If 1+ beginnings have the same point as 1+ endings. You first have to process the beginnings. The old and new ones have per definition a meeting point.

##### Solution:

```python

def solution(A):
    circle_endpoints = []
    for i, a in enumerate(A):
        circle_endpoints += [(i-a, True), (i+a, False)]

    circle_endpoints.sort(key=lambda x: (x[0], not x[1]))

    intersections, active_circles = 0, 0

    for _, is_beginning in circle_endpoints:
        if is_beginning:
            intersections += active_circles
            active_circles += 1
        else:
            active_circles -= 1
        if intersections > 10E6:
            return -1

    return intersections
```
