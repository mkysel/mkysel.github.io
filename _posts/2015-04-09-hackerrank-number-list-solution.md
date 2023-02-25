---
title: "HackerRank 'Number List' Solution"
date: "2015-04-09"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Shashank loves to play with arrays a lot. Today, he has an array A consisting of N positive integers. At first, Shashank listed all the subarrays of his array A on a paper and later replaced all the subarrays on the paper with the maximum element present in the respective subarray.

##### Link

[Number List](https://www.hackerrank.com/challenges/number-list)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

You do not actually need to construct all the sub-arrays, as they reduce to only one element. You also can ignore all sub-arrays, that do not contain elements E > K. I also observed that there are x\*y sub-arrays that match the above specified criteria for each element E > K. X is the distance from E to any previous e > K. Y is the distance from E to the end of the array. This way you crate all the sub-arrays that contain E and are not part of another e.

##### Solution:

```python

#!/usr/bin/py
def numberList(a ,k):
    result = 0

    last_biggest = -1
    a_len = len(a)

    for idx in xrange(a_len):
        if a[idx] > k:
            result += (idx-last_biggest)*(a_len-idx)
            last_biggest = idx

    return result

if __name__ == '__main__':
    t = int(raw_input())
    for _ in xrange(t):
        n,k = map(int, raw_input().split())
        a = map(int, raw_input().split())
        print numberList(a ,k)
```
