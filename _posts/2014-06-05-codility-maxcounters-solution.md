---
title: "Codility 'MaxCounters' Solution"
date: "2014-06-05"
categories: 
  - "c"
  - "codility"
  - "coding-challenge"
---

##### Short Problem Definition:

Calculate the values of counters after applying all alternating operations: increase counter by 1; set value of all counters to current maximum.

##### Link

[MaxCounters](https://codility.com/demo/take-sample-test/max_counters)

##### Complexity:

expected worst-case time complexity is `O(N+M)`

expected worst-case space complexity is `O(N)`

##### Execution:

The idea is to perform the specified operation as stated. It is not required to iterate over the whole array if a new value is set for all the values. Just save the value and check it when an increase on that position is performed.

##### Solution:

```cpp

#include <algorithm>

vector<int> solution(int N, vector<int> &A) {
    vector<int> sol;
    int current_max = 0;
    int last_increase = 0;

    for(int i=0; i<N;i++){
        sol.push_back(0);
    }

    for(unsigned int i=0; i<A.size();i++){
        if (A[i] > N) {
            last_increase = current_max;
        } else {
            sol[A[i]-1] = max(sol[A[i]-1], last_increase);
            sol[A[i]-1]++;
            current_max = max(current_max, sol[A[i]-1]);
        }
    }

    for(int i=0; i<N;i++){
        sol[i] = max(sol[i], last_increase);
    }

    return sol;
}
```
