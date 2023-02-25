---
title: "Codility 'CommonPrimeDivisors' Solution"
date: "2014-08-11"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Check whether two numbers have the same prime divisors.

##### Link

[CommonPrimeDivisors](https://codility.com/demo/take-sample-test/common_prime_divisors)

##### Complexity:

expected worst-case time complexity is O(Z\*log(max(A)+max(B))2);

expected worst-case space complexity is O(1)

##### Execution:

I will post an explaining image soon!

##### Solution:

```python

def gcd(p, q):
  if q == 0:
    return p
  return gcd(q, p % q)

def hasSameFactors(p, q):
    if p == q == 0:
        return True
    
    denom = gcd(p,q)
    
    while (p != 1):
        p_gcd = gcd(p,denom)
        if p_gcd == 1:
            break
        p /= p_gcd
    else:
        while (q != 1):
            q_gcd = gcd(q,denom)
            if q_gcd == 1:
                break
            q /= q_gcd
        else:
            return True
    
    return False


def solution(A, B):
    if len(A) != len(B):
        raise Exception("Invalid input")
    cnt = 0
    for idx in xrange(len(A)):
        if A[idx] < 0 or B[idx] < 0:
            raise Exception("Invalid value")
        if hasSameFactors(A[idx], B[idx]):
            cnt += 1
    
    return cnt
```
