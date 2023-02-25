---
title: "Codility 'PassingCars' Solution"
date: "2014-06-10"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Count the number of passing cars on the road.

##### Link

[PassingCars](https://codility.com/demo/take-sample-test/passing_cars)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(1)

##### Execution:

Count all cars heading in one direction (west). Each car heading the other direction (east) passes all cars that went west so far. Note that east cars at the beginning of the list pass no cars! Also do not forget the upper limit!

##### Solution:

```python

def solution(A):
    west_cars = 0
    cnt_passings = 0

    for idx in xrange(len(A)-1, -1, -1):
        if A[idx] == 0:
            cnt_passings += west_cars
            if cnt_passings > 1000000000:
                return -1
        else:
            west_cars += 1

    return cnt_passings
```
