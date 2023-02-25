---
title: "HackerRank 'Max Min' / 'Angry Children' Solution"
date: "2015-03-01"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Given a list of N integers, your task is to select K integers from the list such that its _unfairness_ is minimized.

##### Link

[Max Min](https://www.hackerrank.com/challenges/angry-children)

##### Complexity:

time complexity is O(N\*log(N));

space complexity is O(N)

##### Execution:

The unfairness is the distance between K elements in a sorted array.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    n = input()
    k = input()
    candies = [input() for _ in range(0,n)]
    candies.sort()
    min_diff = 1000000000
    ## Write code here to compute the answer using (n, k, candies)

    for i in xrange(n - k + 1):
        min_diff = min(min_diff, candies[i+k-1] - candies[i])
    
    print min_diff
```

* * *

```cpp

#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <limits>
using namespace std;

int main() {
    /* The code required to enter n,k, candies is provided*/

    int N, K, unfairness = std::numeric_limits<int>::max();
    cin >> N >> K;
    int candies[N];
    for (int i=0; i<N; i++)
        cin >> candies[i];
    
    sort(candies, candies + N);
        
    for (int i=0; i < N - K + 1; i++){
        unfairness = min(unfairness, candies[i+K-1] - candies[i]);
    }
    
    cout << unfairness << "\n";
    return 0;
}
```
