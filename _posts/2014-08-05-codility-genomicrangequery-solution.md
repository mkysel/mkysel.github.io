---
title: "Codility 'GenomicRangeQuery' Solution"
date: "2014-08-05"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the minimal nucleotide from a range of sequence DNA.

##### Link

[GenomicRangeQuery](https://codility.com/demo/take-sample-test/genomic_range_query)

##### Complexity:

expected worst-case time complexity is O(N+M);

expected worst-case space complexity is O(N)

##### Execution:

Remember the last position on which was the genome (A, C, G, T) was seen. If the distance between Q and P is lower than the distance to the last seen genome, we have found the right candidate.

##### Solution:

```python

def writeCharToList(S, last_seen, c, idx):
    if S[idx] == c:
        last_seen[idx] = idx
    elif idx > 0:
        last_seen[idx] = last_seen[idx -1]

def solution(S, P, Q):
    
    if len(P) != len(Q):
        raise Exception("Invalid input")
    
    last_seen_A = [-1] * len(S)
    last_seen_C = [-1] * len(S)
    last_seen_G = [-1] * len(S)
    last_seen_T = [-1] * len(S)
        
    for idx in xrange(len(S)):
        writeCharToList(S, last_seen_A, 'A', idx)
        writeCharToList(S, last_seen_C, 'C', idx)
        writeCharToList(S, last_seen_G, 'G', idx)
        writeCharToList(S, last_seen_T, 'T', idx)
    
    
    solution = [0] * len(Q)
    
    for idx in xrange(len(Q)):
        if last_seen_A[Q[idx]] >= P[idx]:
            solution[idx] = 1
        elif last_seen_C[Q[idx]] >= P[idx]:
            solution[idx] = 2
        elif last_seen_G[Q[idx]] >= P[idx]:
            solution[idx] = 3
        elif last_seen_T[Q[idx]] >= P[idx]:
            solution[idx] = 4
        else:    
            raise Exception("Should never happen")
        
    return solution
    
```
