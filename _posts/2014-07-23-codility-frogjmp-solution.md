---
title: "Codility 'FrogJmp' Solution"
date: "2014-07-23"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count minimal number of jumps from position X to Y.

##### Link

[FrogJmp](https://codility.com/demo/take-sample-test/frog_jmp)

##### Complexity:

expected worst-case time complexity is O(1);

expected worst-case space complexity is O(1).

##### Execution:

Do not use float division if possible!

##### Solution:

```python

def solution(X, Y, D):
    if Y < X or D <= 0:
        raise Exception("Invalid arguments")
        
    if (Y- X) % D == 0:
        return (Y- X) // D
    else:
        return ((Y- X) // D) + 1
```
