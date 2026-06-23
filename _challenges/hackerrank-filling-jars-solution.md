---
title: "HackerRank 'Filling Jars' Solution"
date: "2015-03-03"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Animesh has _N_ empty candy jars, numbered from 1 to _N_, with infinite capacity. He performs _M_ operations. Each operation is described by 3 integers _a, b_ and _k_. Here, _a_ and _b_ are indices of the jars, and _k_ is the number of candies to be added inside each jar whose index lies between_a_ and _b_ (both inclusive). Can you tell the average number of candies after _M_ operations?

##### Link

[Filling Jars](https://www.hackerrank.com/challenges/filling-jars)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Keep a sum variable. Compute the average at the end.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    n,m = map(int, raw_input().split())

    answer = 0

    for _ in xrange(m):
        a, b, k = map(int, raw_input().split())
        answer += (abs(a-b)+1)*k
    print answer//n
```

* * *

```cpp

 #include<iostream>
 #include<vector>
 #include<math.h>
 #include<array>
 using namespace std;
 int main()
 {
    long n,m;
    long answer=0,t;
    cin>>n>>m;
    t = m;
    while(t--){
        long a,b,k;
        cin>>a>>b>>k;
        answer = answer + (abs(a-b)+1)*k;
    }
    answer = floor(answer/n);
    cout<<answer<<endl;
    return 0;
 }
```
