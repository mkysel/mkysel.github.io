---
title: "Codility 'Fish' Solution"
date: "2014-07-18"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

N voracious fish are moving along a river. Calculate how many fish are alive.

##### Link

[Fish](https://codility.com/demo/take-sample-test/fish)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Put all downstream swimming fishes on a stack. Any upstream swimming fish has to fight(eat) all fishes on the stack. If there is no fish on the stack, the fish survives. If the stack has some downstream fishes at the end, they also survive.

##### Solution:

```python

def solution(A, B):
    survivals = 0
    stack = []
    
    for idx in xrange(len(A)):
        if B[idx] == 0:
            while len(stack) != 0:
                if stack[-1] > A[idx]:
                    break
                else:
                    stack.pop()
                        
            else:
                survivals += 1
        else:
            stack.append(A[idx])
            
    survivals += len(stack)
    
    return survivals
```
