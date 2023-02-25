---
title: "Codility 'MinPerimeterRectangle' Solution"
date: "2014-07-29"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the minimal perimeter of any rectangle whose area equals N.

##### Link

[MinPerimeterRectangle](https://codility.com/demo/take-sample-test/min_perimeter_rectangle)

##### Complexity:

expected worst-case time complexity is `O(sqrt(N))`

expected worst-case space complexity is `O(1).`

##### Execution:

Trivial search for the largest prime.

##### Solution:

```python

import math
def solution(N):
    if N <= 0:
      return 0
  
    for i in xrange(int(math.sqrt(N)), 0, -1):
        if N % i == 0:
            return 2*(i+N/i)
            
    raise Exception("should never reach here!")    
```
