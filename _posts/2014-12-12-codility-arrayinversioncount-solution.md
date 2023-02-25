---
title: "Codility 'ArrayInversionCount' Solution"
date: "2014-12-12"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute number of inversion in an array.

##### Link

[ArrayInversionCount](https://codility.com/demo/take-sample-test/array_inversion_count "Array Inversion Count")

##### Complexity:

expected worst-case time complexity is O(N\*log(N));

expected worst-case space complexity is O(N)

##### Execution:

Any sorting algorithm with a NlogN runtime will do the trick. It is important to count all (remaining) bigger elements on the left side. Do not forget to check for the maximal return value!

##### Solution:

```python

MAX_NR = 1000000000

def mergeSort(alist):
    inversion_count = 0
    if len(alist)>1:
        mid = len(alist)//2
        lefthalf = alist[:mid]
        righthalf = alist[mid:]

        inversion_count += mergeSort(lefthalf)
        if (inversion_count > MAX_NR):
            raise

        inversion_count += mergeSort(righthalf)
        if (inversion_count > MAX_NR):
            raise

        i=0
        j=0
        k=0
        while i<len(lefthalf) and j<len(righthalf):
            if lefthalf[i]<=righthalf[j]:
                alist[k]=lefthalf[i]
                i=i+1
            else:
                alist[k]=righthalf[j]
                j=j+1
                inversion_count += len(lefthalf)-i
                if (inversion_count > MAX_NR):
                    raise
            k=k+1

        while i<len(lefthalf):
            alist[k]=lefthalf[i]
            i=i+1
            k=k+1

        while j<len(righthalf):
            alist[k]=righthalf[j]
            j=j+1
            k=k+1

    return inversion_count

def solution(A):
    try:
        return mergeSort(A)
    except:
        return -1
```
