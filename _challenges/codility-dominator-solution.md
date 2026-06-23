---
title: "Codility 'Dominator' Solution"
date: "2014-08-08"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find an index of an array such that its value occurs at more than half of indices in the array.

##### Link

[Dominator](https://codility.com/demo/take-sample-test/dominator)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(1)`

##### Execution:

As explained in the training material...

##### Solution:

```python

def solution(A):
    candidate_ele = ''
    candidate_cnt = 0
    
    for value in A:
        if candidate_ele == '':
            candidate_ele = value
            candidate_cnt = 1
        else:
            if value != candidate_ele:
                candidate_cnt -= 1
                if candidate_cnt == 0:
                    candidate_ele = ''
            else:
                candidate_cnt += 1
        
    if candidate_cnt == 0:
        return -1
        
    cnt = 0
    last_idx = 0
    
    for idx, value in enumerate(A):
        if value == candidate_ele:
            cnt += 1
            last_idx = idx
            
    if cnt > len(A)//2:
        return last_idx
        
    return -1
```
