---
title: "HackerRank 'Possible Path' Solution"
date: "2020-10-25"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Adam is standing at point (a,b) in an infinite 2D grid. He wants to know if he can reach point (x,y) or not. The only operation he can do is to move to point (a+b, b) (a, b+a) (a-b, b), or (a, b-a) from some point (a,b). It is given that he can move to any point on this 2D grid, i.e., the points having positive or negative (or ) co-ordinates.

Tell Adam whether he can reach  or not.

##### Link

[Possible Path](https://www.hackerrank.com/challenges/possible-path/problem)

##### Complexity:

time complexity is O(log(N))

space complexity is O(1)

##### Execution:

The problem statement is not requesting the path from A,B to X,Y, just that a path exists. This makes it a mathematical problem.

We know that GCD(a, b) == GCD(a+b, b) == GCD(a-b, b). Why? Because if something is a divisor D of B, adding/subtracting B from any number A that is also divisible by D will not change the divisibility requirement.

Hence GCD(a, b) == GCD(a+n\*b, b+m\*a) where n, m are integers.

Following the rules, we can realize that GCD(a+n\*b, b+m\*a) is GCD(x, y), which leads us to the final solution.

##### Solution:

```python
def gcd(a, b):
    if a % b == 0:
        return b
    else:
        return gcd(b, a % b)

def solve(a, b, x, y):
    return gcd(a,b) == gcd(x,y)
```
