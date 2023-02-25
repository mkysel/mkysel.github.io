---
title: "Codility 'FibFrog' Solution"
date: "2014-12-28"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count the minimum number of jumps required for a frog to get to the other side of a river.

##### Link

[FibFrog](https://codility.com/demo/take-sample-test/fib_frog)

##### Complexity:

expected worst-case time complexity is O(N\*log(N))

expected worst-case space complexity is O(N)

##### Execution:

This problem can be solved by in a Dynamic Programming way. You need to know the optimal count of jumps that can reach a given leaf. You get those by either reaching the leaf from the first shore or by reaching it from another leaf.

The N\*log(N) time complexity is given by the fact, that there are approximately log(N) Fibonacci numbers up to N and you visit each position once.

As for the sequence hack: there are 26 Fibonacci numbers smaller than 100k, so I just preallocate an array of this size.

##### Solution:

```python

def get_fib_seq_up_to_n(N):
    # there are 26 numbers smaller than 100k
    fib = [0] * (27)
    fib[1] = 1
    for i in xrange(2, 27):
        fib[i] = fib[i - 1] + fib[i - 2]
        if fib[i] > N:
            return fib[2:i]
        else:
            last_valid = i
    
    
    
def solution(A):
    # you can always step on the other shore, this simplifies the algorithm
    A.append(1)

    fib_set = get_fib_seq_up_to_n(len(A))
    
    # this array will hold the optimal jump count that reaches this index
    reachable = [-1] * (len(A))
    
    # get the leafs that can be reached from the starting shore
    for jump in fib_set:
        if A[jump-1] == 1:
            reachable[jump-1] = 1
    
    # iterate all the positions until you reach the other shore
    for idx in xrange(len(A)):
        # ignore non-leafs and already found paths
        if A[idx] == 0 or reachable[idx] > 0:
            continue

        # get the optimal jump count to reach this leaf
        min_idx = -1
        min_value = 100000
        for jump in fib_set:
            previous_idx = idx - jump
            if previous_idx < 0:
                break
            if reachable[previous_idx] > 0 and min_value > reachable[previous_idx]:
                min_value = reachable[previous_idx]
                min_idx = previous_idx
        if min_idx != -1:
            reachable[idx] = min_value +1

    return reachable[len(A)-1]
```
