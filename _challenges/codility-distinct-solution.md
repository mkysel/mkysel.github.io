---
title: "Codility 'Distinct' Solution"
date: "2014-08-01"
categories: 
  - "c"
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute number of distinct values in an array.

##### Link

[Distinct](https://codility.com/demo/take-sample-test/distinct)

##### Complexity:

expected worst-case time complexity is `O(N \* log(N))`

expected worst-case space complexity is `O(N)`

##### Execution:

Sorting in both C++ and Python takes N log N time. We know that for this particular problem sorting the array will be the dominant runtime complexity. This is the case for the second python solution. What about the other ones?

The first solution is a neat pythonic way of solving a distinct entries problem. The set is implemented as a hash table so it is possible that it will degrade to a linked list. Therefore the actual worst case would be N^2.

This is not the case with C++ (as code 3 shows). The std set is a Red-Black Tree and therefore has insertion complexity of log N. (overall N log N)

##### Solution:

```python

def solution(A):
    return len(set(A))
```

* * *

```python

def solution(A):
    if len(A) == 0:
        return 0

    A.sort()

    nr_values = 1
    last_value = A[0]

    for idx in xrange(1, len(A)):
        if A[idx] != last_value:
            nr_values += 1
            last_value = A[idx]

    return nr_values
```

* * *

```cpp

#include <set>
int solution(const vector<int> &A) {
    std::set<int> theSet;

    for(int idx = 0; idx < A.size(); idx++){
        theSet.insert(A[idx]);
    }

    return theSet.size();
}
```
