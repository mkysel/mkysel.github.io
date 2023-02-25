---
title: "Codility 'CannonBalls' 2012 Chi Solution"
date: "2015-05-07"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Simulate a cannon shooting and heaps of falling cannonballs

##### Link

[Cannon Balls](https://codility.com/c/run/demoBTUPTW-UMX)

##### Complexity:

expected worst-case time complexity is `O(H+M+N)`

expected worst-case space complexity is `O(H+M)`

##### Execution:

The obvious brute force solution would be to check the minimum index that is high enough to block the shot. This would result in a N\*M runtime. Based on observation we can see that the hit location can be precomputed and it changes only with small steps.

- \-1 in the hit\_location means, that the ball either ricochets or flies above the highest peak
- anything that hits the 0th index also does nothing

##### Solution:

```python

def solution(A, B):
    highest_ball = max(B)
    hit_location = [-1] * (highest_ball+1)
    ricochet = A[0]
    
    for idx, a in enumerate(A):
        lvl = min(a, highest_ball)
        while hit_location[lvl] == -1 and lvl > ricochet:
            hit_location[lvl] = idx
            lvl -= 1
            
    # print hit_location

    for ball in B:
        hits_at = hit_location[ball]
        # print "ball", ball, "hits at", hits_at
        if hits_at <= 0:
            continue
        A[hits_at-1] += 1
        hit_location[A[hits_at-1]] = min(hit_location[A[hits_at-1]], hits_at-1)
        
        
    return A
```
