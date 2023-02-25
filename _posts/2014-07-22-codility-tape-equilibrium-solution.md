---
title: "Codility 'Tape Equilibrium' Solution"
date: "2014-07-22"
categories: 
  - "c"
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Minimize the value |(A\[0\] + ... + A\[P-1\]) - (A\[P\] + ... + A\[N-1\])|.

##### Link

[TapeEquilibrium](https://codility.com/demo/take-sample-test/tape_equilibrium)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(N)`

##### Execution:

In the first run I compute the left part up to the point i and the overall sum _last_. Then I compute the minimal difference between 0..i and i+1..n.

##### Solution:

```python

import sys

def solution(A):
    #1st pass
    parts = [0] * len(A)
    parts[0] = A[0]
 
    for idx in xrange(1, len(A)):
        parts[idx] = A[idx] + parts[idx-1]
 
    #2nd pass
    solution = sys.maxint
    for idx in xrange(0, len(parts)-1):
        solution = min(solution,abs(parts[-1] - 2 * parts[idx]));  
 
    return solution
```

* * *

```cpp

#include <algorithm>
#include <climits>

int solution(vector<int> &A) {
    unsigned int size = A.size();

    vector<long long> parts;
    parts.reserve(size+1);

    long long last = 0;

    for (unsigned int i=0; i<size-1; i++){
        if (i == 0){
            parts.push_back(A[i]);
        } else {
            parts.push_back(A[i]+parts[i-1]);
        }
        if (i==size-2){
            last = parts[i]+ A[i+1];
        }
    }

    long long solution = LLONG_MAX;

    for(unsigned int i=0; i<parts.size(); i++){
        solution = min(solution,abs(last - 2 * parts[i]));
    }

    return solution;
}
```
