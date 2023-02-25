---
title: "Codility 'PermCheck' Solution"
date: "2014-05-22"
categories: 
  - "c"
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Check whether array N is a permutation.

##### Link

[PermCheck](https://codility.com/demo/take-sample-test/perm_check)

##### Complexity:

expected worst-case time complexity is O(N);

expected worst-case space complexity is O(N)

##### Execution:

Mark elements as seen in a boolean array. Elements seen twice or out of bounds of the size indicate that the list is no permutation. The check if the boolean array only contains true elements is not required. This solution only works with permutations starting from 1.

##### Solution:

```python

def solution(A):
    seen = [False] * len(A)

    for value in A:
        if 0 <= value > len(A):
            return 0
        if seen[value-1] == True:
            return 0
        seen[value-1] = True

    return 1
```

* * *

```cpp

int solution(vector<int> &A) {
    // the use of a auto_ptr would be better, but it does not work with arrays
    // use of boost::scoped_array seems like an overkill
    bool * seen = new bool[A.size()];
    
    for (unsigned int i=0; i< A.size(); i++){
        seen[i] = false;
    }    
    
    for (unsigned int i=0; i< A.size(); i++){
        if (!(0 < A[i] && A[i] <=A.size() && seen[A[i]-1] == false)){
            delete[] seen;
            return 0;
        } else {
            seen[A[i]-1] = true;
        }
    }
    
    delete[] seen;
    return 1;
}
```
