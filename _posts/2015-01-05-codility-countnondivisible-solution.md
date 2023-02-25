---
title: "Codility 'CountNonDivisible' Solution"
date: "2015-01-05"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Calculate the number of elements of an array that are not divisors of each element.

##### Link

[CountNonDivisible](https://codility.com/demo/take-sample-test/count_non_divisible)

##### Complexity:

expected worst-case time complexity is O(N\*log(N));

expected worst-case space complexity is O(N)

##### Execution:

Using the Sieve of Eratosthenes, you generate divisors for all input elements of A. If a given number x is a divisor of element _(x\*N == element)_, then N also is a divisor. _(N = element//x)._ After all divisors are computed, we simply subtract those (multiplied by their counts or 0) from the total number of elements in A.

##### Solution:

```python

def solution(A):
 
    A_max = max(A)
 
    count = {}
    for element in A:
        if element not in count:
            count[element] = 1
        else:
            count[element] += 1
 
    divisors = {}
    for element in A:
        divisors[element] = set([1, element])
 
    # start the Sieve of Eratosthenes
    divisor = 2
    while divisor*divisor <= A_max:
        element_candidate = divisor
        while element_candidate  <= A_max:
            if element_candidate in divisors and not divisor in divisors[element_candidate]:
                divisors[element_candidate].add(divisor)
                divisors[element_candidate].add(element_candidate//divisor)
            element_candidate += divisor
        divisor += 1
 
    result = [0] * len(A)
    for idx, element in enumerate(A):
        result[idx] = (len(A)-sum([count.get(divisor,0) for divisor in divisors[element]]))
 
    return result
```
