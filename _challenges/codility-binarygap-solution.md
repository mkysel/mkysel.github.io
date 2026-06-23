---
title: "Codility 'BinaryGap' Solution"
date: "2014-08-02"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find longest sequence of zeros in binary representation of an integer.

##### Link

[BinaryGap](https://codility.com/demo/take-sample-test/binary_gap)

##### Complexity:

expected worst-case time complexity is `O(log(N))`

expected worst-case space complexity is `O(1)`

##### Execution:

The solution is straight-forward! Use of binary shift.

##### Solution:

```python

def solution(N):
    cnt = 0
    result = 0
    found_one = False

    i = N    
        
    while i:
        if i & 1 == 1:
            if (found_one == False):
                found_one = True
            else:
                result = max(result,cnt)
            cnt = 0
        else:
            cnt += 1
        i >>= 1
   
    return result
```
