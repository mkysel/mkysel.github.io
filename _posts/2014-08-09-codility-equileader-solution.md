---
title: "Codility 'EquiLeader' Solution"
date: "2014-08-09"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Find the index S such that the leaders of the sequences A\[0\], A\[1\], ..., A\[S\] and A\[S + 1\], A\[S + 2\], ..., A\[N - 1\] are the same.

##### Link

[EquiLeader](https://codility.com/demo/take-sample-test/equi_leader)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Get the leader as in the training material. Afterwards check every position if both sides have enough leader occurrences.

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
        return 0

    cnt = 0
    last_idx = 0

    for idx, value in enumerate(A):
        if value == candidate_ele:
            cnt += 1
            last_idx = idx

    if cnt < len(A)//2:
        return 0

    equi_cnt = 0
    cnt_to_the_left = 0
    for idx, value in enumerate(A):
        if value == candidate_ele:
            cnt_to_the_left +=1
        if cnt_to_the_left > (idx + 1)//2 and \
            cnt - cnt_to_the_left > (len(A) - idx - 1) //2:
            equi_cnt += 1

    return equi_cnt
```
