---
title: "HackerRank 'Cut the sticks' Solution"
date: "2015-03-17"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given N sticks, where each stick has the length of a positive integer. A cut operation is performed on the sticks such that all of them are reduced by the length of the smallest stick.

##### Link

[Cut the sticks](https://www.hackerrank.com/challenges/cut-the-sticks)

##### Complexity:

time complexity is `O(n\*log(n))`

space complexity is `O(n)`

##### Execution:

Cutting every stick will result in O(N^2) which is not required. Sorting the array requires nlogn time. Python pop() requires O(1) time and importantly it does not rearrange or reallocate the array.

With each step, remove all elements from the list (actually a stack/queue) that have the same value and print the size of the remaining list.

##### Solution:

```python

#!/usr/bin/py
def computeSticks(arr):
    arr.sort(reverse=True)
    while len(arr) > 0:
        print len(arr)
        block_cut = arr.pop()
        while len(arr) > 0 and arr[-1] <= block_cut:
            arr.pop()

if __name__ == '__main__':
    n = int(raw_input())
    arr = map(int, raw_input().split())
    computeSticks(arr)
```
